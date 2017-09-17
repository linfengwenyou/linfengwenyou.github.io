### 标签学习

#### input标签

包含的比较多：button checkbox color date datetime datetime-local email file hidden image month number password  等

```
<input type="button" onclick="" value="xxx" />
```

#### area

area对象代表图像映射的一个区域（图像映射指的是带有可点击区域的图像）

```
<area shape="rect" coords="0,0,110,260"
onMouseOver="writeText('太阳和类似木星这样的气态行星是到目前为止太阳系中最大的物体。')"
href ="/example/html/sun.html" target ="_blank" alt="Sun" />
// 从某一区域上设置监听
```



<a href="http://www.w3school.com.cn/jsref/dom_obj_anchor.asp" target="_blank">更多标签</a>



### 正则表达式使用

正则表达式涉及到的方向：

* 判断格式是否正确
* 搜索并存储需要的内容
* 文本替换

语法

```
／正则表达式主体/[修饰符]				// 修饰符可选
```

或

```
new RegExp("表达式主题","修饰符")	 // 
```



示例：

```
var patt = /demo/i						// 搜索demo，i表示搜索不区分大小写
var regex = new RegExp("demo","ig")		 // 全局，且忽略大小写
```



#### 判读格式是否正确—test

```
// 判断格式是否正确
	function isDecimal(strValue)
	{
	// 注意正则表达式的格式/regex/,尤其注意不是用引号包裹
		var regexp = /^\d+\.\d+$/			
		return regexp.test(strValue)
	}
```

#### 搜索字符串— search/exec

* 搜索位置

```
// 搜索字符串位置，只会搜索一个，查到后就不查找了
function myFunction(){
	var str = "Visit Runoob! 哈哈哈 runoob new this place"
	var n = str.search(/Runoob/i)
	document.write(n)
}
```

* 搜索字符，存在就返回，不存在就返回null   __与 test功能比较相似__

```
function myFunction(){
	var str = "Visit Runoob! 哈哈哈 runoob new this place"
	var regex = /e/
	var tmp = regex.exec(str)
	document.write(tmp)
}
```



#### 匹配字符串—match

```
// 将所有符合条件的字符串查找出来，并存放到数组
function myFunction(){
	var str = "Visit Runoob! 哈哈哈 runoob new this place"
	var regex = /oo/g
	var tmp = str.match(regex)
	document.write(tmp)
}
```



#### 替换字符串—replace

```
// 替换字符串, 注意替换是生成新的字符串，只会替换第一个
function myFunction(){
	var str = "Visit Runoob! 哈哈哈 runoob new this place"
	var newStr = str.replace("Runoob","Microsoft")
	document.write(newStr)
}
```



#### 更改正则表达式（重新编译正则表达式）— compile

在脚本执行中，可能需要根据条件更改某一正则表达式

```
function myFunction(){
	var str = "Visit Runoob! 哈哈哈 runoob new this place"
	var regex = new RegExp("e")				// 匹配e，且只匹配第一个
	var tmp = str.match(regex)
	regex.compile("o","g")					// 更改正则表达式规则，匹配o,并且全局匹配查找，有几个查几个
	tmp = str.match(regex)
	document.write(tmp)
}
```

