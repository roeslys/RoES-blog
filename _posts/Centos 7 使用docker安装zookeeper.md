## <center>CentOS 7 ʹ��Docker�Zookeeper</center>

#### 1.  ǰ����Docker�Ѿ���װ����

> û�а�װ�Ŀ��Կ���ƪ����-->[CentOS 7��װDocker]()



#### 2.��ȡZookeeper����

```shell
docker pull zookeeper
#ʹ������鿴��ȡ���ľ���
docker images
```



#### 3.����Zookeeper

```shell
#��������������Ŀ¼
mkdir -p /home/service/zookeeper/conf
mkdir -p /home/service/zookeeper/data
mkdir -p /home/service/zookeeper/log

docker run -d --name zk --restart always zookeeper

#�������е������ļ����Ƴ���
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
��ɣ������˿ڷֱ��ǣ�zookeeper�ͻ��˶˿ڣ�����˿ڣ�ѡ��˿�