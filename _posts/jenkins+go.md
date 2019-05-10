**docker+jenkins+golang持续集成持续交付（CI/CD）**

最近因公司发展需要，增加了一些go语言开发，对项目要求使用jenkins+go+docker自动部署上线。

## 一、安装jenkins

1、安装Jenkins，详情见centos使用docker搭建jenkins，jenkins使用方法见jenkins的安装和使用

2、jenkins安装go插件，**Go plugin**

安装该插件，点击 “系统管理” -> “管理插件” -> “可选插件” -> 选择 “Go Plugin” -> 点击最下边 “直接安装” 即可完成安装。

3、配置go插件

系统管理” -> “Global Tool Configuration” -> “Go” -> “新增 Go”

![1556177007802](C:\Users\buhuo02\AppData\Roaming\Typora\typora-user-images\1556177007802.png)

## 二、在搭建jenkins的服务器和要构建上传的应用服务器均安装go环境

1、下载安装包

下载地址：[https://golang.org/dl/](https://golang.org/dl/)

```
wget [https://dl.google.com/go/go1.12.4.linux-amd64.tar.gz](https://dl.google.com/go/go1.12.4.linux-amd64.tar.gz)
```
2、解压缩安装

```
tar -C /home/service -zxvf go1.12.4.linux-amd64.tar.gz
```
3、配置环境变量

```
vim /etc/profile
export GOROOT=/home/service/go
export PATH=$PATH:$GOROOT/bin
source /etc/profile
```
4、测试

```
[root@bogon /]# go version
go version go1.12.4 linux/amd64
```

## 三、jenkins配置

1、新建任务go选择构建一个自由风格的软件项目，java选择构建一个maven项目

![1556177266170](C:\Users\buhuo02\AppData\Roaming\Typora\typora-user-images\1556177266170.png)

2、配置git路径和git账号密码和分支，或者git密钥![1556177362234](C:\Users\buhuo02\AppData\Roaming\Typora\typora-user-images\1556177362234.png)

3、构建环境选择

![1556177419249](C:\Users\buhuo02\AppData\Roaming\Typora\typora-user-images\1556177419249.png)

4、构建执行shell，先选择执行shell，再选择ssh。

![1556177468792](C:\Users\buhuo02\AppData\Roaming\Typora\typora-user-images\1556177468792.png)

![1556177446915](C:\Users\buhuo02\AppData\Roaming\Typora\typora-user-images\1556177446915.png)



```
# 配置 GOPATH

export GOPATH="$JENKINS_HOME/golang_workspace"
export GOBINPATH="$GOPATH/bin"
export PATH="$PATH:$GOBINPATH"
export GO111MODULE=on
export GOPROXY=https://goproxy.io
#export GOPROXY="https://athens.azurefd.net"

# 执行 go get & build 命令

#创建 $GOPATH目录
mkdir -p $GOPATH

# 输出当前时间

date
#构建可执行二进制文件
CGO_ENABLED=0 GOARCH=amd64 go build server.go
#将可执行二进制文件传输到应用服务器
#ssh root@172.16.3.41 'bash -x -s' < /home/golang/bh-go-server-user/run.sh
scp server root@172.16.3.41:/home/golang/bh-go-server-user/
scp config/server.pem root@172.16.3.41:/home/golang/bh-go-server-user/config
scp config/server.key root@172.16.3.41:/home/golang/bh-go-server-user/config
```

![1556177608458](C:\Users\buhuo02\AppData\Roaming\Typora\typora-user-images\1556177608458.png)

## 四、应用服务器配置

1、在上传到服务器的路径下创建config目录

```
[root@bogon bh-go-server-user]# tree
.
├── config
│   ├── config.env	#配置启动可执行二进制文件的配置文件
│   ├── server.key	#openssl
│   └── server.pem	#openssl
├── run.sh		#运行脚本
└── server		#可执行二进制文件

1 directory, 5 files


```

2、配置config.env

![1556177965656](C:\Users\buhuo02\AppData\Roaming\Typora\typora-user-images\1556177965656.png)

3、在shell出已经用scp将二进制文件server和pem和key传输到了应用服务器

4、执行二进制文件

./server

5、利用run.sh脚本执行二进制文件

