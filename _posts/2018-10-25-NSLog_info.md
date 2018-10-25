#### NSLog 打印格式

```
2018-10-25 15:11:54.605569+0800 DemoTestSlider[2440:46651] log message

# 解析：
# 2018-10-25 15:11:54.605569+0800			时间戳信息
# DemoTestSlider							工程名
# [2440:46651]								[进程ID:线程ID]
# log message								自定义的打印信息
```



时间戳，工程名，自定义打印信息可以很方便的验证出来，[进程id:线程id]就先验证一下

##### 查看进程ID

* 模拟器运行项目【方便查看】
* 打开终端输入命令

```
$ top 
```

命令执行效果如图

![NSLog_top命令打印](https://linfengwenyou.github.io/images/2018-10-25-top_images.png)

通过途中可以看出PID为 3639,此时查看下日志输出中进程ID匹配上

```
2018-10-25 15:28:50.670354+0800 DemoTestSlider[3639:71759] libMobileGestalt 
```



##### 查看线程ID

> 通过上面的日志打印可以看出线程id应该为71759，在这里我们打开断点【点击查看视图层级】。使用LLDB操作

```
(lldb) thread info
thread #1: tid = 0x1184f, 0x000000010d028c2a libsystem_kernel.dylib`mach_msg_trap + 10, queue = 'com.apple.main-thread', stop reason = signal SIGSTOP
```

由上述信息可以看出tid为0x1184f，将其转换为10进制为：71759  正好符合操作。



> 通过命令查看【后续补充】

```
# 会打印出不少信息，暂时打印这几个
sudo dtruss -ap 3936					
	PID/THRD  RELATIVE  ELAPSD    CPU SYSCALL(args) 		 = return
 3936/0x143df:       460      86      0 __disable_threadsignal(0x1, 0x0, 0x0)		 = 0 0
 3936/0x14310:      1302      89      1 __disable_threadsignal(0x1, 0x0, 0x0)		 = 0 0
 3936/0x14221:   1954269      52     43 stat64("/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/CoreSimulator/Profiles/Runtimes/iOS.simruntime/Contents/Resources/RuntimeRoot/System/Library/PrivateFrameworks/UIKitCore.framework\0", 0x7FFEE41367A0, 0x0)		 = 0 0
 
....

```



##### 知道这些有什么用？

* 首先满足好奇心
* 知道了进程ID 在使用模拟器需要杀掉app模拟Home键不行时可以直接kill PID
* 知道一个进程开了多少线程，可以大概了解到一个app资源消耗情况。



#### Ubantu下查看线程数量

```
top 命令直接看不到
cat /proc/PID/status
```

