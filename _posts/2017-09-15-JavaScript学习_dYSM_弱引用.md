#### 数组的基本操作

* 输出数组内容

```
var tmpArr=["one","two","three","four"]
for (index in tmpArr)					// 需要注意，这里取出的index是索引，并不是数组的内容
{
document.write(tmpArr[index] + "</br>")
}
/*
one
two
three
four
*/
```

* 数组合并

```
var tmpArr=["one","two","three","four"]
var tmpArr2=["first","second","third","fourth"]
var newTmpArr=tmpArr.concat(tmpArr2)			// 方法是concat
document.write(newTmpArr)
/* 
one,two,three,four,first,second,third,fourth
*/
```

* 将数组变成字符串输出

```
var tmpArr=["one","two","three","four"]
document.write(tmpArr.join("-"))			// 如果使用join(),默认以"，"分割
/*
one-two-three-four
*/
```

* 数组排序

```
// 字符排序
var tmpArr=["one","two","three","four"]
document.write(tmpArr.sort())
/*
four,one,three,two
*/


// 数字排序
function sortNumber(a,b)
{
return a - b
}
var arr=["10","5","40","45","21","53","48","36"]
document.write(arr.sort(sortNumber))
```



<a href="http://www.w3school.com.cn/jsref/jsref_obj_array.asp" target="_blank">更多数组方法</a>



#### Dysm配置使用

```
dwarfdump --uuid XX.app/XX			// 查看app的uuid
dwarfdump --uuid XX.dysm			// 查看dysm的uuid

grep 'XX arm64' xxyy.crash			// 查看打印的地址,<>中的地址就是uuid，需要保持与上面两个一样就可以解析符号信息了
// 0x10009c000 - 0x10043bfff XINCHANGBank arm64  <3cc531cbf0f53262a2ea2798ccaa441c> 
```

配置，首先找到`symbolicatecrash`

```
find /Applications/Xcode.app -name symbolicatecrash -type f
```

可以将`symbolicatecrash`，`dSYM`,`.crash`文件拷贝到桌面文件夹，然后执行命令

```
./symbolicatecrash 12.crash 12.dSYM > symbol.crash// 生成的崩溃文件，格式可以随便写

// 如果出现错误 Error: "DEVELOPER_DIR" is not defined at ./symbolicatecrash line 69.
// 则是没有配置，执行如下命令
export DEVELOPER_DIR=/Applications/XCode.app/Contents/Developer // 如果想永久有效，就添加到 ~/.bash_profile
```



#### NSHashTable

NSHashTable 可以用来做弱引用容器  使用NSMapTable作为delegate弱引用的集合再适合不过了，与使用NSProxy弱引用添加到数组或字典效果一致。

```

```

<a href="http://www.cocoachina.com/ios/20150519/11809.html" target="_blank">更多资料</a>
