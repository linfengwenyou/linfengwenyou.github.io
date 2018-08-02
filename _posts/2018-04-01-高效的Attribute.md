#### Attribute
`__attribute__`是一个高级的编译器指令，它允许开发者置顶更多的编译检查和一些高级的编译器优化。

指令有以下三种：

> * 函数属性
> * 类型属性
> * 变量属性



##### 常用的几个操作如下

```
// 带描述信息的API弃用
#define __deprecated_msg(_msg) __attribute__((deprecated(_msg)))
 
// 遇到__unavailable的变量/方法，编译器直接抛出Error，可以用于API更新
#define __unavailable   __attribute__((unavailable))
 
// 如果不使用方法的返回值，进行警告
#define __result_use_check __attribute__((__warn_unused_result__))

```



示例：

```
// 属性使用， 这种使用可以快速的查找到所有使用到这个属性或方法的地方，方便快速的进行功能改进，在isue navigator 中，搜索msg即可
@property (strong, nonatomic) UIImage *defaultImage __deprecated_msg("此属性废弃，请使用XXXimage");

- (UIImage *)currentImage  __deprecated_msg("此方法废弃，请使用属性xxx来获取image信息");

// 此时使用defaultImage会提示编译出错信息
@property (strong, nonatomic) UIImage *defaultImage __unavailable;


```


#### Clang警告处理

```
#pragma clang diagnostic push			// 对当前编译环境进行压栈
#pragma clang diagnostic ignored "-Wundeclared-selector"   // 忽略对未声明的selector警告
///代码
#pragma clang diagnostic pop			// 对编译环境进行出栈

```



##### 常见的可以忽略的警告

```
// 可以在Build Settings中搜索warn 查找所有的警告配置信息

```

