##### docker zookeeper集群

三台主机：

* 主机一：172.16.3.52

* 主机一：172.16.3.63

* 主机一：172.16.3.64

1、下载镜像

```
docker pull zookeeper:3.4.12
```

2、在主机上建立挂载目录和zookeeper配置文件

```
mkdir -p /home/data/zookeeper_data/conf \
mkdir -p /home/data/zookeeper_data/data \
cd /home/data/zookeeper_data/conf
touch zoo.cfg
vi zoo.cfg
```

三台主机上的zoo.cfg配置信息如下：

```
clientPort=12181
dataDir=/data
dataLogDir=/data/log
tickTime=2000
initLimit=5
syncLimit=2
autopurge.snapRetainCount=3
autopurge.purgeInterval=0
maxClientCnxns=60
server.0=172.16.3.52:2888:3888
server.1=172.16.3.40:2888:3888
server.2=172.16.3.41:2888:3888
```

3、在主机一上为自己分配server id，命令如下：

```
cd /home/data/zookeeper_data/data
echo "0" > myid
```

在主机二上为自己分配server id，命令如下：

```
cd /home/data/zookeeper_data/data
echo "1" > myid
```

在主机二上为自己分配server id，命令如下：

```
cd /home/data/zookeeper_data/data
echo "2" > myid
```

4、三台主机依次启动容器：

```
docker run  --restart=always --network host -p 12181:2181 -p 2888:2888 -p 3888:3888 -v /home/data/zookeeper_data/data:/data -v /home/data/zookeeper_data/conf:/conf --name zookeeper -d zookeeper:3.4.12
```

命令说明：

- --network host: 使用主机上的网络配置，如果不用这种模式，而用默认的bridge模式，会导致容器跨主机间通信失败
- -v /data/zookeeper_data/data:/data：主机的数据目录挂载到容器/data下
- -v /data/zookeeper_data/conf:/conf： 主机的配置目录挂载到容器的/conf下，容器内的zkServer.sh默认会读取/conf/zoo.cfg下的配置

都启动完成后，每台主机的2181/2888/3888端口都会开放出来了

5、验证

```
docker exec -it 容器id /bin/bash
```

```
docker logs 容器id/容器名称
```

```
echo stat|nc 172.16.3.52
```

