### 判断远端是否安装了ssh客户端





#### linux查看系统版本

```
cat /proc/version
```



#### Centos python升级

```
# 选择合适的python版本
wget https://www.python.org/ftp/python/3.7.3/Python-3.7.3.tgz

# 解压
tar -zxf Python-3.7.3.tgz

# 安装依赖包
yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gcc  libffi-devel

# 编译、安装、运行
cd Python-3.7.3
./configure --prefix=/usr/local/python3.7
make && make install

# Python3.7上的版本，需要安装个依赖包
yum install -y libffi-devel


# 备份原来2的版本
mv /usr/bin/python /usr/bin/python.bak

# 创建新的软连接
ln -s /usr/local/python/bin/python3.7 /usr/bin/python		# 需要找到自己的可执行文件位置

# 查看版本即可
python -V


## 注意 yum 需要使用Python2, 需要改下yum配置
vim /usr/bin/yum
vim /usr/libexec/urlgrabber-ext-down

将文件头的#!/usr/bin/python改为#!/usr/bin/python2即可
```



### Git安装

```
yum -y install git

```



### 配置仓库

```
# 码云管理
# 生成公钥
ssh-keygen -t rsa -C 'xxxx@sina.com'

# 查看生成的公钥  【整体拷贝即可】
➜  ~ cat .ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC9PyBPNcB7pj42/ZeG5txudqija2kRFc2f76abq0sdIfZOz6yNjM2AXXO5d6j2zP58UWKG8V1oWYd1JOICo0//K5rFjlYW63mh2eNiwdvhWbGYaWDfIxqQtS/a/zKaHYOQcAKx2TpLXuTmOdWmXWCkmaa2sKmP0Yfejxxxxxxxx1Ju0WL57GSl6iHu8v9gwRIAk+Nujx5vazRjyoYhviGftkcJl1p/F5hLcG8KOV7XRpn0CB8V3MuYLw10vNGeRhMyGZ+kmwuyM4Ek2czlBU1/cvEJTFMVeQtVT4UenCDua5x0edjfUHAxTv1wj6t xxxxx@sina.com

# 添加主机到本机SSH可信列表
ssh -T git@gitee.com

# 成功后的提示
The authenticity of host 'gitee.com (116.211.167.14)' can't be established.
ECDSA key fingerprint is SHA256:FQGC9Kn/eye1W8icdBgrQp+KkGYoFgbVr17bmjey0Wc.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'gitee.com,116.211.167.14' (ECDSA) to the list of known hosts.
Hi XXX! You've successfully authenticated, but GITEE.COM does not provide shell access.


```







#### Python安装服务器

```
小戴博客：http://www.linuxyw.com  # 运维牛人
```









---

### 问题一：

```
ssh: connect to host 2001:19f0:5401:20bf:5400::fe11 port 22: No route to host
```

可能问题：  [最难定位]

1. 服务端未开机或其他问题





### 问题二：

ssh 经常连接不上，需要看看是不是服务端口关闭了？

需要处理一个问题，扫描端口







### 问题三：

Python服务器搭建





### 地址推荐

#### vps展示

https://www.vpsss.net/vps-tuijian



