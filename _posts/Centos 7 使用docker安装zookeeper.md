## <center>CentOS 7 使用Docker搭建Zookeeper</center>

#### 1.  前提是Docker已经安装好了

> 没有安装的可以看这篇文章-->[CentOS 7安装Docker]()



#### 2.拉取Zookeeper镜像

```shell
docker pull zookeeper
#使用命令查看拉取到的镜像
docker images
```



#### 3.启动Zookeeper

```shell
#创建宿主机挂载目录
mkdir -p /home/service/zookeeper/conf
mkdir -p /home/service/zookeeper/data
mkdir -p /home/service/zookeeper/log

docker run -d --name zk --restart always zookeeper

#把容器中的配置文件复制出来
docker cp -a zk:/conf/zoo.cfg /home/service/zookeeper/conf/zoo.cfg

docker stop zk
docker rm zk
docker run -d --name zk --restart always \
-p 2181:2181 -p 2888:2888 -p 3888:3888 \
-v /home/service/zookeeper/conf/zoo.cfg:/conf/zoo.cfg \
-v /home/service/zookeeper/data:/data \
-v /home/service/zookeeper/log:/datalog \
zookeeper
```
docker run -d --name zk --restart always -p 2181:2181 -p 2888:2888 -p 3888:3888 -v /home/service/zookeeper/conf/zoo.cfg:/conf/zoo.cfg -v /home/service/zookeeper/data:/data -v /home/service/zookeeper/log:/datalog zookeeper
完成，三个端口分别是：zookeeper客户端端口，跟随端口，选择端口