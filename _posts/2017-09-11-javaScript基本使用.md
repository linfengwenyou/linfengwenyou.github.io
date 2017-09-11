JavaScript是属于网络的脚本语言。JavaScript被数百万的网页用来改进设计、验证表单、监测浏览器、创建cookies,及更多的应用。

主要记录下常见的语法结构，其编成思想并不受语言限制。

### 输出方式

#### 写入HTML

```
// document.write只能在HTML输出中使用，如果在文档加载后使用该方法，会覆盖整个文档
// 不像其他的函数必须调用才回执行，只要放到<script></script>中就会在执行到时执行
document.write("<h1>This is a heading</h1>");		// 输出一个h标签
```

#### 对事件作出响应

```
// 并没有被<script></script>包裹，onclick的值就是一个脚本操作
<button type="button" onclick="alert('Welcome!')">点击这里</button>

// 补充 button的事件
// onload 		用户进入页面触发 
// onunload 	用户离开页面触发
// onchange 	改变输入字段的内容时触发
// onmouseover	用户鼠标移至HTML元素上方触发
// onmouseout	用户鼠标移开某元素时触发
// onmousedown	鼠标按下触发
// onmouseup	释放鼠标触发
// onclick		完成点击鼠标时
```

#### 改变HTML的内容

```
// 根据id查找指定元素，然后修改元素展示的值
x=document.getElementById("demo")  //查找元素
x.innerHTML="Hello JavaScript";    //改变内容


// 补充 改变元素的属性
document.getElementById(id).attribute=new Value; // 更改属性， 如src ，innerHTML

// 改变HTML元素的样式
document.getElementById(id).style.property=new style;// color 
```

#### 弹框的几种样式

#### 查找HTML元素的几种方式

```
var x=document.getElementById("main")		// 查找id="main"的元素， 内部可能有多个子标签
var y=x.getElementsByTagName("p");			// 通过标签名查找元素
```



### 基本类型

#### 字符串

需要使用引号包围

```
var str1="str1"; 		// 等号两边可以有空格，也可以没有，按照脚本的写法以后就不写空格，养成习惯

// 最外层的字符决定字符的区域,一下两种方式都可以表示一个字符串
var carname2 = 'Bill "Gates"';			// Bill "Gates"
var answer2="He is called 'Bill'";		// He is called 'Bill'
```

常用方法／属性方法调用

```
var str1="str1"
str1.length;	// 返回字符串的长度，因为不是一个方法是一个属性，不需要加（)
```



```
var str1="str1"
str1.toUpperCase();	// 转成大写的  str1并不会变成大写，只是缓存中的变成大写了
```

`+`用作拼接字符

```
var name = 'law'+'son_y';
```



#### 数字

JavaScript只有一种数字类型。数字可以带小数点，也可以不带：

```
var x1=340.00;
var x2=34;
var y=123e5;		// 有没有分号都可以，如果与其他脚本语言保持一致可以不使用
var z=2134e-5
```



#### 布尔

条件测试中用的比较多

```
var x=true
var y=false
```

#### 数组

数组的创建有三种创建方式

```
var cars=new Array()
cars[0]="Audi"
cars[1]="BMW"
...

```

或

```
var cars=new Array("Audi","BMW")
```

再或

```
var cars=["Audi","BMW"];
```



#### 对象

JavaScript没有字典，是直接用映射关系来表示对象。。？

创建对象也有多种方式

```
var person={firstname:"liu",lastname:"song",age:26}
```

或

```
var person={
  firstname:"lius",
  lastname:"song",
  age:26
}
```

或

```
person=new Object();
person.firstname="Bill";
person.lastname="Gates";
person.age=56;
```

或 使用对象构造器

```
function person(firstname,lastname,age,eyecolor)
{
this.firstname=firstname;
this.lastname=lastname;
this.age=age;
this.eyecolor=eyecolor;
}

var myFather=new person("Bill","Gates",56,"blue"); // 通过构造器创建对象
```



调用方式

```
person.firstname				// 这个比较像对象的调用方式
var name=person["firstname"]	 // 这点就像是根据key-value操作了
```



__与OC中的字典有极大的相似，不需要特殊声明，需要什么属性直接赋值。__

方法的声明

```
function person(firstname,lastname,age,eyecolor)
{
this.firstname=firstname;
this.lastname=lastname;
this.age=age;
this.eyecolor=eyecolor;

this.changeName=changeName;
function changeName(name)			// 定义方法，在对象的构造器中
{
this.lastname=name;
}
}
```

方法的调用

```
person.doSomething()
```



#### Null 和 Undefined

Undefined表示变量没有值【如果只声明没有赋值就是这种情况】

可以通过将变量的值设置为null来清空变量。

```
var demo;						// 这样不赋值打印就是Undefined

var demo="demo test";			 // 赋值
demo=null;						// demo被清空,打印就是null
```



#### 变量类型设置

返回一个默认初始化的值，不是Undefined 也不是null

```
var person=new Object;
var str=new String;	
var isOK=new Boolean;
var cars=new Array;
var i=new Number;
```



#### 一个特殊的比较运算符

```
// === 全等（值或类型），可用作类型比较
x=5
x===5;		// true
x==="5";	// false
```

其他的运算符，【比较运算符／条件运算符，三目运算都是与其他高级语言一样的】



#### 标签的使用

给一段代码取一个别名，方便进行跳出等处理。使用情况：

非循环，但是需要在某种情况下不执行其他代码，其实相当于`if`操作



```
cars=["BMW","Volvo","Saab","Ford"];
demo:								// 为下面代码起个别名
{
document.write(cars[0] + "<br>"); 
document.write(cars[1] + "<br>");
break demo;							// 执行到这时跳出这个代码块，少一个都不行
document.write(cars[2] + "<br>"); 
document.write(cars[3] + "<br>"); 
document.write(cars[4] + "<br>"); 
document.write(cars[5] + "<br>"); 
}
document.write("this is a test <br>");

/**输出
BMW
Volvo
this is a test
*/
```



#### with的使用

由来：平时如果我们需要调用一个对象的属性或者方法时用 `obj.attritute`  或 `obj.methon()`; 但是有时同一个对象好多属性要一起调用，这个时候一个个的用点语法是要累死掉。所以。。就有了`with`语法。

```

// 操作
<script type="text/javaScript">
var person={
  firstname:"lius",
  lastname:"song",
  age:26
}
with(person) {
  txt=firstname + lastname + age;			// 可以直接读取person对象的属性方法
}
</script>
```



### 方法及流程

#### 函数

```
// 一般是放倒<script></script>块中
// 格式：
function functionname() // 如果有参 functionname(argument1, argument2)
{
  // 需要执行的代码
}
```

示例：

* 无参，无返回值

```
function simpleFunction()
{
document.write("this is a test</br >")
}
```

* 有参无返回值

```
function myFunction(name,job)
{
alert("Welcome " + name + ", the " + job);
}
```

* 有参有返回值

```
function simpleFunction(name,job)
{
return name + "'s job is "+ job;  // 如果在某种情况下想退出当前函数可以使用 return;与其他高级语言方式相同
}
```

  

调用方式：

```
<button onclick="myFunction('Bill Gates','CEO')">点击这里</button>
<button onclick="alert(simpleFunction('lawson_y','Developer'))">点击执行</button>
```



#### 流程控制

与高级语言一样，`if`格式

```
if(condition)
{
  // 执行代码
} else if(condition) {
  // 执行代码
} else {
  // 执行代码
}
```

`switch`格式

```
switch(n)
{
  case 1:
  // 执行块
  break;
  case 2:
  // 执行块
  brea;
  default:	// 请使用 default 关键词来规定匹配不存在时做的事情
}
```



while 格式

```
while(condition){}

// 或
do {}
while(condition);
```



for 格式

```
for(var i=0;i<100;i++)
{}

// 或
var person={firstname:"john",lastname:"doe",age:25}   // 遍历对象的属性，超像字典，只是OC的字典没有这种遍历方式
for(x in person)
{
  txt=txt+person[x];
}
```



### 错误输出

```
try {
  // 运行代码
  if(x=="xxx") throw "not fit the situation";
  ...
}
catcher(err){  // 返回throw的描述内容
  
}
```





### 小知识点

* 脚本只要用`<script></script>`包裹就行，放在`header`与`body`中并不影响。
* 如果脚本是在外面需要配置如下

```
<script type="text/javascript" src="xxx.js"></script>
```

* JavaScript对大小写敏感，使用时需要注意
* 注释与OC代码一样使用`//`或`/**/`
* 每行代码尾部可以有分号`:`作为一句话结束【如果两句话在一行必须有分号】
* 注意与C语言相似【顺序执行】，所有调用者必须在方法实现之后。如果有函数调用则要看调用方式是加载调用还是用户手动触发，手动触发就不用计较这些了。
* 函数内的变量都是局部变量，函数执行完就会删除
* 函数外声明的变量为全局变量，网页上的所有脚本和函数都能访问它。 在页面关闭后删除。
* 向未声明的JavaScript变量来分配值：如果把值赋给尚未声明的变量，该变量将会被自动作为全局变量声明。

```
carname="Volvo";			// 将声明一个全局变量，即时它在函数内执行
```



* break一般用于跳出循环，跳出循环后悔继续执行循环后的代码【如果有的话】
* continue 终止当前循环，开始下一个