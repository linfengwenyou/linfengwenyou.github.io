### 需求点

* 网络图片异步下载，不能阻塞主线程
* 频繁刷新，网络延迟【操作缓存（可变字典）】
* 图片缓冲区(可变字典)【最后通过沙盒存储】
* 沙盒存储



#### 涉及到的技术点



##### 别忘记初始化

````
 self.ops = [[NSMutableDictionary alloc] init];
````

```
- (NSOperationQueue *)queue
{
    if (!_queue) {
        _queue = [[NSOperationQueue alloc] init];
        _queue.maxConcurrentOperationCount = 3;
    }
    return _queue;
}
```



##### `json`数据源获取

```
- (NSArray *)dataSource
{
    if (!_dataSource) {
        
        NSData *data = [NSData dataWithContentsOfFile:[[NSBundle mainBundle] pathForResource:@"apps.json" ofType:nil]];
        _dataSource = [NSJSONSerialization JSONObjectWithData:data options:NSJSONReadingMutableContainers error:NULL];
    }
    return _dataSource;
}
```

##### 沙盒路径

```

- (NSString *)getFilePathName:(NSDictionary *)tmpDict
{
    // 写入沙盒
    NSString *path = [NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES) lastObject];
    // 获取文件名
    NSString *fileName = [tmpDict[@"icon"] lastPathComponent];
    NSString *filePath = [path stringByAppendingPathComponent:fileName];
    return filePath;
}

```



##### `cell`配置

```
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    
    NSDictionary *tmpDict = _dataSource[indexPath.row];
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:NSStringFromClass([UITableViewCell class])];

    NSString *filePath = [self getFilePathName:tmpDict];
    UIImage *image = [UIImage imageWithData:[NSData dataWithContentsOfFile:filePath]];   // 通过缓存获取图片，获取不到就重新下载
    
    if (!image) {
        image = [UIImage imageNamed:@"user_default"];
        
        NSOperation *tmpOperation = [self.ops objectForKey:tmpDict[@"icon"]];  // 如果有operation就不需要再创建了
        if (!tmpOperation) {
            // 创建一个队列，来下载图片，通过key值保存这个操作，防止因为网络延迟导致频繁创建操作
            NSOperation *operation = [NSBlockOperation blockOperationWithBlock:^{
//                if (indexPath.row > 5) {  // 可以用来模拟网络慢的情况
//                    [NSThread sleepForTimeInterval:15.0f];
//                }
                NSData *data = [NSData dataWithContentsOfURL:[NSURL URLWithString:tmpDict[@"icon"]]];
                
                [[NSOperationQueue mainQueue] addOperationWithBlock:^{  // 切换到主队列刷新界面
                    [data writeToFile:filePath atomically:YES];   // 写入缓存文件

                    UIImage *image = [UIImage imageWithData:data];
                    cell.imageView.image = image;
                    
                    [self.ops removeObjectForKey:tmpDict[@"icon"]]; // 下载成功后从操作缓存中移除数据
                    [tableView reloadRowsAtIndexPaths:@[indexPath] withRowAnimation:UITableViewRowAnimationFade];  // 刷新指定的cell，更新
                }];
            }];
            
            [self.ops setObject:operation forKey:tmpDict[@"icon"]];  // 配置缓存，下次如果有，就不再创建operation
            [self.queue addOperation:operation];    // 将操作添加到队列中并执行
        } else {
            NSLog(@"已经创建了操作，不需要再次创建了");
        }
    }

    cell.imageView.image = image;
    cell.textLabel.text = tmpDict[@"name"];
    return cell;
}
```



##### 内存警告记得清除

```
- (void)didReceiveMemoryWarning
{
    [super didReceiveMemoryWarning];
    
    [_ops removeAllObjects];
    [_queue cancelAllOperations];
}
```



