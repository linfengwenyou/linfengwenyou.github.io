### LLDB调试

#### 打印命令

```
p		打印基本类型
po      打印对象类型
```



#### 调用方法

```
call	call可以使用在没有返回值，不需要显示输出的情况下，如设置view颜色
call [self.view setBackgroundColor:[UIColor redColor]];
// 这样就不用为了看改下颜色的效果再进行一次build
```



#### 表达式

```
expr (e)    常用于调试时修改变量的值
```



#### 镜像地址

```
image lookup --address 寻址，定位异常代码的位置，某一个文件的行数
eg: image lookup --address 0x00000001097c393f
```

```
image lookup --type 查看类型所包含的属性等
eg: image lookup --type UIImage
```



#### 堆栈信息查看

```
bt		打印调用层级，可以用来回溯调用流程
```



#### 断点

##### 普通断点

```
直接通过界面操作，或者使用br命令添加断点
eg: br -s 
```

##### 异常断点

```
通过界面操作，当出现异常时会在异常处进行拦截
```

##### 符号断点

> 根据符号来进行全局打断点，作用域是整个工程

```
# 格式为：
- [name_of_the_class name_of_the_method:]
eg:
-[UINavigationItem setLeftBarButtonItem:]
```

> 而且处理异常断点也很有用
>
> 示例如下

```
# 1. 添加符号断点：
UIViewAlertForUnsatisfiableConstraints

# 2. 卡住后在事件处操作,下面会打印出布局树【会有提示AMBIGUOUS（模棱两可的，模糊不清的）】，再配合异常信息几本就可以定位了
po [[UIWindow keyWindow] _autolayoutTrace]

```



##### 观察断点

```
watchPoint 配置
```



#### chisel使用

##### 安装

```
 # 1. 安装
 brew install chisel
 
 # 2. 在~/.lldbinit文件中添加一行，没有就创建这个文件
 ==> Caveats
Add the following line to ~/.lldbinit to load chisel when Xcode launches:
  command script import /usr/local/opt/chisel/libexec/fblldb.py
```

##### 递归打印view

```
po [view recursiveDescription]
pviews view
```

##### 递归打印控制器

```
pvc
```

##### 控件/控制器搜索

```
fv scrollView
fv home

// 支持正则搜索
eg:
(lldb) fv UI\\w+
0x7fef91d05b80 UIWindow
0x7fef91c0c8e0 UIView
0x7fef91c09320 UIView
0x7fef91c096d0 UILabel
0x7fef91c0a2b0 UIImageView
0x7fef91c0ca80 _UILayoutGuide
0x7fef91c0d590 _UILayoutGuide

(lldb) fv UI\\w+l
0x7fef91c096d0 UILabel

(lldb) fv UI\\we
0x7fef91c0ca80 _UILayoutGuide
0x7fef91c0d590 _UILayoutGuide

(lldb) fv UI\\w+e
0x7fef91c0c8e0 UIView
0x7fef91c09320 UIView
0x7fef91c096d0 UILabel
0x7fef91c0a2b0 UIImageView
0x7fef91c0ca80 _UILayoutGuide
0x7fef91c0d590 _UILayoutGuide

(lldb) fv view
0x7fef91c0c8e0 UIView
0x7fef91c09320 UIView
0x7fef91c0a2b0 UIImageView

(lldb) fv Image\\w*
0x7fef91c0a2b0 UIImageView
```

##### 边界样式调整

```
border -c blue -w 2 0x7feae2d605f0  //添加边框
unborder 0x7feae2d605f0   //移除边框
```



##### 显示或隐藏指定的view

```
show view
hide view
```



##### 重新渲染

```
caflush   // 与[CATransaction flush] 功能一样
```



##### 打断点

> 这个命令就是用来打断点用的了，虽然大家断点可能都喜欢在图形界面里面打，但是考虑一种情况：我们想在 [MyViewController viewWillAppear:] 里面打断点，但是 MyViewController并没有实现 viewWillAppear: 方法， 以往的作法可能就是在子类中实现下viewWillAppear:，然后打断点，然后rebuild。

> 那么幸好有了 bmessage命令。我们可以不用这样就可以打这个效果的断点：

```
(lldb) bmessage -[MyViewController viewWillAppear:]
```

上面命令会在其父类的 `viewWillAppear: `方法中打断点，并添加上了条件：

```
[self isKindOfClass:[MyViewController class]]
```





#### 逆向容易用到的

##### 查看方法的内存地址

```
_shortMethodDescription

eg:
po [ViewController _shotMethodDescription]
```



##### 查看属性的内存地址

```
_ivarDescription

eg:
po [obj _ivarDescription]   // obj 为一个对象
```



##### 对内存地址打断点

```
br s -a address
```



### 问题记录

#### libc++abi.dylib: terminate_handler unexpectedly threw an exception

这类问题，一般属于，数组或字典类型或有空对象导致。

可以通过设置断点来处理：

> 工程->show the breakpoint navigator—>(右下角"+")Exception BreakPoint..—>右键编辑 【将All改成C++】 就会捕获所有C++异常。



问题发生原因：Xcode9添加资源文件没有添加到xcproj文件中，即没有正确添加引用。

