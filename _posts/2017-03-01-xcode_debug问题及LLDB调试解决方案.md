### LLDB调试

#### 打印命令

```
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
eg:
e a = 10;
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

#### 查看地址信息

```
 // NSString *a = @"zzzzzzzzz";
 p a	# 打印对象信息，同事打印出指针地址
 p/t a  # 打印对象信息，使用二进制表示指针地址
 p/u a  # 打印对象信息，使用十进制表示指针地址
 
eg:
    NSString *a = @"demotest";
    NSString *b = [a mutableCopy];
    NSString *c = [b copy];
    
(lldb) p a
(__NSCFConstantString *) $0 = 0x000000010fbbd268 @"demotest"
(lldb) p b
(__NSCFString *) $1 = 0x000060000025db80 @"demotest"
(lldb) p c
(NSTaggedPointerString *) $2 = 0xa002801831003048 @"demotest"
(lldb) p/u c
(NSTaggedPointerString *) $3 = 11529918837411557448 @"demotest"	// 这个长整型，可以直接用来打印：NSLog(@"%@",11529918837411557448UL);  demotest
```

##### 为什么一串数字可以打印出demotest?

<a href="http://www.cocoachina.com/ios/20150918/13449.html" target="_blank">资料</a>

下面引用些内容

> 对象在内存中是对齐的，它们的地址总是指针大小的整数倍，通常为16的倍数。对象指针是一个64位的整数，而为了对齐，一些位将永远是零。
>
> 为了存储优化内存节约，提升性能才加入NSTaggedPointer类型的。通过运行时来处理的方式。不能直接放到内存是 为了兼容不同架构体系。只能在运行时时才可以由系统优化为NSTaggedPointer类型。
>
> Tagged Pointer有一个简单的应用，那就是NSNumber。它使用60位来存储熟知。最低位置1为NSNumber的标志。在这个例子中，就可以存储任何所需内存小于60位的数值。



>  优化体现点： 
>
> 从外部看Tagged Pointer很像一个对象。它能够响应消息，因为objc_msgSend可以识别Tagged Pointer.假设你调用integerValue，它将从那60位中提取数值并返回。这样，每访问一个对象，就省下了一次真正对象的内存分配，省下了一次间接取值的时间。同事引用计数可以是空指令，因为没有内存释放。对于常用的类，这将是一个巨大的性能提升。
>
> NSString长度可变，又可远远超过60位，为此对于哪些内存小于60位的字符串，它可以创建一个Tagged Pointer。其余的则放置在真正的NSString对象里。这样使得短字符串的性能得到明显的提升。



```
NSLog(@"%@",11529918837411557448UL);  
// 打印出demotest
```



 __格式展示如下__

```
x 按十六进制格式显示变量。
d 按十进制格式显示变量。
u 按十六进制格式显示无符号整型。
o 按八进制格式显示变量。
t 按二进制格式显示变量。
a 按十六进制格式显示变量。
c 按字符格式显示变量。
f 按浮点数格式显示变量。
```

<a href="http://blog.csdn.net/yasi_xi/article/details/9263955" target="_blank">地址命令</a>

#### 查看静态库符号表

```
nm -a /System/Library/Frameworks/Foundation.framework/Versions/C/Foundation

nm xxx.a | grep xxSymbol
```



#### 断点

##### 普通断点

```
直接通过界面操作，或者使用br命令添加断点
eg: 
br set -n main  # 对某个方法设置断点
```

##### 异常断点

```
通过界面操作，当出现异常时会在异常处进行拦截
```

##### 符号断点

> 根据符号来进行全局打断点，作用域是整个工程

```
# 格式为：
- [name_of_the_class name_of_the_method:]		# 类方法或对象方法（+／-）
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

> 可以在某个变量被写入/读取是暂停程序运行，相当于在setter和getter处打了断点

```
# 1. 在xcode lldb调试时，手动设置watchpoint
# 2. 命令操作
watchpoint set variable -w read _abc4
watchpoint set variable -w read_write _abc4

eg:
watchpoint set variable self->_infoModel			// 需要使用->进行指针操作
```

> 当前arm和x86上，一次最多允许创建4个watchpoint，继续创建会提示问题

<a><href="http://www.dreamingwish.com/article/lldb-usage-a.html" target="_blank">延伸阅读1</a>

<a><href="http://blog.csdn.net/quanqinyang/article/details/51321338" target="_blank">延伸阅读2</a>



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



##### dis 查看方法的汇编指令

```
- (void)testAction1
{
	NSLog(@"%@",11529918837411557448UL);			// 断点
}

(lldb) dis
9-13`-[ViewController testAction1]:
    0x10ee2a7a0 <+0>:  pushq  %rbp
    0x10ee2a7a1 <+1>:  movq   %rsp, %rbp
    0x10ee2a7a4 <+4>:  subq   $0x10, %rsp
    0x10ee2a7a8 <+8>:  leaq   0x4a99(%rip), %rax        ; @"%@"
    0x10ee2a7af <+15>: movabsq $-0x5ffd7fe7ceffcfb8, %rcx ; imm = 0xA002801831003048 
    0x10ee2a7b9 <+25>: movq   %rdi, -0x8(%rbp)
    0x10ee2a7bd <+29>: movq   %rsi, -0x10(%rbp)
->  0x10ee2a7c1 <+33>: movq   %rax, %rdi
    0x10ee2a7c4 <+36>: movq   %rcx, %rsi
    0x10ee2a7c7 <+39>: movb   $0x0, %al
    0x10ee2a7c9 <+41>: callq  0x10ee2cf80               ; symbol stub for: NSLog
    0x10ee2a7ce <+46>: addq   $0x10, %rsp
    0x10ee2a7d2 <+50>: popq   %rbp
    0x10ee2a7d3 <+51>: retq   
    0x10ee2a7d4 <+52>: nopw   %cs:(%rax,%rax)
```







### 问题记录

#### libc++abi.dylib: terminate_handler unexpectedly threw an exception

这类问题，一般属于，数组或字典类型或有空对象导致。

可以通过设置断点来处理：

> 工程->show the breakpoint navigator—>(右下角"+")Exception BreakPoint..—>右键编辑 【将All改成C++】 就会捕获所有C++异常。



问题发生原因：Xcode9添加资源文件没有添加到xcproj文件中，即没有正确添加引用。

