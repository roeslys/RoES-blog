1、进入/etc/rc.d/init.d/目录创建zookeeper文件

```shell
cd /etc/rc.d/init.d/
vim zookeepeer
```

2、zookeeper文件

```shell
#!/bin/bash
#chkconfig:2345 20 90
#description:zookeeper
#processname:zookeeper
export JAVA_HOME=/home/service/jdk1.8.0_161
case $1 in
        start) su root /home/service/zookeeper-3.4.12/bin/zkServer.sh start;;
        stop) su root /home/service/zookeeper-3.4.12/bin/zkServer.sh stop;;
        status) su root /home/service/zookeeper-3.4.12/bin/zkServer.sh status;;
        restart) su /home/service/zookeeper-3.4.12/bin/zkServer.sh restart;;
        *) echo "require start|stop|status|restart" ;;

esac
```

3、测试

```shell
[root@bogon init.d]# service zookeeper stop
ZooKeeper JMX enabled by default
Using config: /home/service/zookeeper-3.4.12/bin/../conf/zoo.cfg
Stopping zookeeper ... STOPPED
[root@bogon init.d]# service zookeeper status
ZooKeeper JMX enabled by default
Using config: /home/service/zookeeper-3.4.12/bin/../conf/zoo.cfg
Error contacting service. It is probably not running.
[root@bogon init.d]# service zookeeper start
ZooKeeper JMX enabled by default
Using config: /home/service/zookeeper-3.4.12/bin/../conf/zoo.cfg
Starting zookeeper ... ^[[ASTARTED
[root@bogon init.d]# service zookeeper status
ZooKeeper JMX enabled by default
Using config: /home/service/zookeeper-3.4.12/bin/../conf/zoo.cfg
Mode: standalone
[root@bogon init.d]# ps -ef|grep zookeeper
root      2656     1 11 11:33 pts/0    00:00:03 /home/service/jdk1.8.0_161/bin/java -Dzookeeper.log.dir=. -Dzookeeper.root.logger=INFO,CONSOLE -cp /home/service/zookeeper-3.4.12/bin/../build/classes:/home/service/zookeeper-3.4.12/bin/../build/lib/*.jar:/home/service/zookeeper-3.4.12/bin/../lib/slf4j-log4j12-1.7.25.jar:/home/servicezookeeper-3.4.12/bin/../lib/slf4j-api-1.7.25.jar:/home/service/zookeeper-3.4.12/bin/../lib/netty-3.10.6.Final.jar:/home/service/zookeeper-3.4.12/bin/../lib/log4j-1.2.17.jar:/home/service/zookeeper-3.4.12/bin/../lib/jline-0.9.94.jar:/home/service/zookeeper-3.4.12/bin/../lib/audience-annotations-0.5.0.jar:/home/service/zookeeper-3.4.12/bin/../zookeeper-3.4.12.jar:/home/service/zookeeper-3.4.12/bin/../src/java/lib/*.jar:/home/service/zookeeper-3.4.12/bin/../conf: -Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.local.only=false org.apache.zookeeper.server.quorum.QuorumPeerMain /home/service/zookeeper-3.4.12/bin/../conf/zoo.cfg
root      2737  1832  0 11:34 pts/0    00:00:00 grep --color=auto zookeeper
```

4、加入到来及自启动

```
chkconfig --add zookeeper
```

5、测试

```shell
[root@bogon init.d]# chkconfig --list

mysqld         	0:关	1:关	2:开	3:开	4:开	5:开	6:关
netconsole     	0:关	1:关	2:关	3:关	4:关	5:关	6:关
network        	0:关	1:关	2:开	3:开	4:开	5:开	6:关
php-fpm        	0:关	1:关	2:开	3:开	4:开	5:开	6:关
zookeeper      	0:关	1:关	2:开	3:开	4:开	5:开	6:关
```

