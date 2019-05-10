1、下载地址
旧版本地址链接
http://archive.apache.org/dist/zookeeper/
稳定版本地址
http://mirrors.shu.edu.cn/apache/zookeeper/?C=N;O=A
2、解压缩
$ tar -zxvf zookeeper-3.4.5.tar.gz
3、在zookeeper的解压目录下创建一个配置文件 conf/zoo.cfg
内容如下：

tickTime=2000
dataDir=/home/service/zookeeper-3.4.5/data
dataLogDir=/home/service/zookeeper-3.4.5/logs
clientPort=2181
4、启动程序
./zkServer.sh start

