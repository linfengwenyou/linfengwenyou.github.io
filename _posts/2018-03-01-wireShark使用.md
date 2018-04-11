# wireShark

###  抓一个http包，分析抓到的Json数据，请求参数等信息

#### 1. 如何筛选抓取到的数据【过滤器的使用很重要】

开始使用时，面对大量的封包信息很容易就怯场了，所以学会筛选有效信息很有用	

```
# 在filter栏上填写表达式
ip.src == 192.168.1.102 or ip.dst == 192.168.1.102
```

#### 2. 过滤表达式的规则

* 协议过滤

```
tcp
http
dns 
...
```



* IP过滤

```
ip.src == xx.xx.xx.xx   源地址为
ip.dst == xx.xx.xx.xx	目标地址
```



* 端口过滤

```
tcp.port == 80 		端口为80的
tcp.srcport == 80	只显示TCP协议的源端口为80的
```



* Http模式过滤

```
// 根据请求类型
http.request.method == "POST"	可以快速的将整个需要的网络请求给过滤出来

// 根据接口地址
http.request.uri contains "/api/special/get_home_data"

// 复合查询
http.request.method == "GET" && http contains "Host:"
http contains "HTTP/1.1 200 OK" && http contains "Content-Type"
```



* 逻辑运算符AND / OR

```
// matches(匹配)和contains(包含某字符串)语法
ip.src == 192.168.1.102 AND tcp contains "GET"
ip.src == xx.xx.xx.xx AND tcp matches "GET"
```



<a href="https://blog.csdn.net/hzhsan/article/details/43453251" target = "_blank">wireShark过滤文档查询</a>



###  抓https包，分析抓到的Json数据，请求参数等信息

> 为了安全考虑，wireshark只能查看封包，而不能修改封包的内容，或者发送封包。

###  抓去TCP/UDP协议包，抓取QQ和微信的数据

#### 1. 配置抓取手机网络请求

> USB连接线
>
> 1 查找手机的UUID 
>
> 2 rvictl -s <UUID>  // 运行完这条命令后就会生成一个rvi0的虚拟端口，直接抓取这个端口的数据即可
>
> 3 用完后，最好是关闭
>
> rvictl -x <UUID>

###  修改抓取到的QQ和微信的数据，并进行有效传输

> 为了安全考虑，wireshark只能查看封包，而不能修改封包的内容，或者发送封包。  意思是不能直接修改了吗