1、进入/etc/init.d目录并创建redis二进制文件

```
cd /etc/init.d

vim redis
```

2、redis文件内容如下

例如安装路径为/usr/redis/redis-3.2.4，复制该路径下的 /utils/redis_init_scri文件内容带redis里。

```shell
[root@bogon utils]# pwd
/home/redis-4.0.8/utils
[root@bogon utils]# ls
build-static-symbols.tcl  cluster_fail_time.tcl  corrupt_rdb.c  create-cluster  generate-command-help.rb  graphs  hashtable  hyperloglog  install_server.sh  lru  redis-copy.rb  `redis_init_script`  redis_init_script.tpl  redis-sha1.rb  releasetools  speed-regression.tcl  whatisdoing.sh
```

注意修改EXEC，CLIEXEC配置为实际安装路径

```
#!/bin/sh

# chkconfig: 2345 10 90  

# description: Start and Stop redis   

REDISPORT=6379
EXEC=/usr/redis/redis-3.2.4/src/redis-server
CLIEXEC=/usr/redis/redis-3.2.4/src/redis-cli

PIDFILE=/var/run/redis_${REDISPORT}.pid
CONF="/usr/redis/redis-3.2.4/redis.conf"

case "$1" in
    start)
        if [ -f $PIDFILE ]
        then
                echo "$PIDFILE exists, process is already running or crashed"
        else
                echo "Starting Redis server..."
                $EXEC $CONF &
        fi
        ;;
    stop)
        if [ ! -f $PIDFILE ]
        then
                echo "$PIDFILE does not exist, process is not running"
        else
                PID=$(cat $PIDFILE)
                echo "Stopping ..."
                $CLIEXEC -p $REDISPORT shutdown
                while [ -x /proc/${PID} ]
                do
                    echo "Waiting for Redis to shutdown ..."
                    sleep 1
                done
                echo "Redis stopped"
        fi
        ;;
    restart)
        "$0" stop
        sleep 3
        "$0" start
        ;;
    *)
        echo "Please use start or stop or restart as first argument"
        ;;
esac
```

3、测试

```shell
[root@bogon init.d]#chkconfig redis on
[root@bogon init.d]# chkconfig --list
netconsole     	0:关	1:关	2:关	3:关	4:关	5:关	6:关
network        	0:关	1:关	2:开	3:开	4:开	5:开	6:关
redis          	0:关	1:关	2:开	3:开	4:开	5:开	6:关
zookeeper      	0:关	1:关	2:开	3:开	4:开	5:开	6:关
```

