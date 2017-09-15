#### libc++abi.dylib: terminate_handler unexpectedly threw an exception

这类问题，一般属于，数组或字典类型或有空对象导致。

可以通过设置断点来处理：

> 工程->show the breakpoint navigator—>(右下角"+")Exception BreakPoint..—>右键编辑 【将All改成C++】 就会捕获所有C++异常。



问题发生原因：Xcode9添加资源文件没有添加到xcproj文件中，即没有正确添加引用。

