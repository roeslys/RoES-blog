1、为了是conf配置文件更加简洁，当域名多了不会混乱。每个域名一个conf配置文件
include             /data/nginx/conf/vhost/*.conf;
2、在，每个域名配置前添加备注，并分隔开来，加#注释掉即可
#-------------www.baidu.com.conf----------------------------
3、设置301跳转，使得http跳到https，这样看起来就想一个网站
server
    {
        listen 80;
        server_name  www.buhuokeji.com buhuokeji.com;
        index index.html index.htm index.php default.html default.htm default.php;
        root /home/www/buhuokeji.com;
        return 301 https://www.baidu.com$request_uri;
}
4、添加404网页
server
{
listen 80;
server_name www.baidu.com; #绑定域名
error_page 404 /404.html;
} 
5、设置nginx开机自启动
cd /lib/systemd/system/
vim nginx.service

[Unit]
Description=nginx 
After=network.target 
   
[Service] 
Type=forking 
ExecStart=/usr/local/nginx/sbin/nginx
ExecReload=/usr/local/nginx/sbin/nginx reload
ExecStop=/usr/local/nginx/sbin/nginx quit
PrivateTmp=true 
   
[Install] 
WantedBy=multi-user.target

systemctl enable nginx.service 设置开机启动

killall nginx
systemctl start nginx.service    启动nginx
systemctl stop nginx.service    结束nginx
systemctl restart nginx.service    重启nginx

6、设置禅道开机自启动配置
cd /lib/systemd/system/
vim zentao.service

[Unit]
Description=zentao
After=network.target

[Service]
Type=forking
ExecStart=/opt/zbox/zbox start
ExecReload=/opt/zbox/zbox restart
ExecStop=/opt/zbox/zbox stop
PrivateTmp=true

[Install]
WantedBy=multi-user.target

启动zentao服务：
systemctl start zentao.service
设置开机自启动：
systemctl enable zentao.service
停止开机自启动：
systemctl disable zentao.service
查看服务当前状态：
systemctl status zentao.service
重新启动服务：
systemctl restart zentao.service
查看所有已启动的服务：
systemctl list-units --type=service

7、设置redis开机启动
vi /usr/local/redis/redis.conf
设置doemonize yes
复制redis.conf到/etc/redis/6379.conf
mkdir /etc/redis
cp /usr/local/redis/redis.conf /etc/redis/6379
复制redis启动脚本到/etc/init.d/redis
find / -name redis_init_script
cp /usr/redis/redis-3.2.4//utils/redis_init_script /etc/init.d/redis
修改启动参数
vi /etc/init.d/redis
在/bin/bash下添加注释
# chkconfig: 2345 10 90  
# description: Start and Stop redis

REDISPORT=6379
EXEC=/usr/local/redis/bin/redis-server
CLIEXEC=/usr/local/redis/bin/redis-cli

PIDFILE=/var/run/redis_${REDISPORT}.pid
CONF="/etc/redis/${REDISPORT}.conf"

启动redis
打开redis命令:service redis start
关闭redis命令:service redis stop
设为开机启动:chkconfig redis on
设为开机关闭:chkconfig redis off



8、jenkins开机启动脚本
vim /etc/init.d/jenkins
#! /bin/bash
#chkconfig:345 80 80      
#345是安全级别,80 80 是启动响应优先级 
#description:Jenkins
#开机启动时描述

#Jenkins的存放位置，文件夹名称是tomcat-jenkins(后面会用到)
jenkinspath=/data/tomcat-jenkins/

#加载函数库
. /etc/init.d/functions
#判断路径是否存在
if [ ! -d "$jenkinspath" ] ; then
    echo "cannot find jenkins path"
    exit 0
fi

if (($# != 1)); then
    echo "Usage:$0 {start|stop|restart}"
    exit 0
fi

case "$1" in
  status)
    status=`ps -ef |grep -v grep |grep tomcat-jenkins |wc -l`
    if(($status==1));then
        action "jenkins is running" /bin/true
        exit 0
    fi
    if(($status==0));then
        action "jenkins is not running" /bin/false
        exit 0
    fi
    ;;    
  start)
    status=`ps -ef |grep -v grep |grep tomcat-jenkins |wc -l`
    if(($status==1));then    
        echo "jenkins has been started"
        exit 0
    fi    
    sh  $jenkinspath/bin/startup.sh >& /dev/null 
    [ $? -eq 0 ] && action "jenkins is starting" /bin/true ||\
    action "jenkins start failure" /bin/false
    ;;
  stop)
    status=`ps -ef |grep -v grep |grep tomcat-jenkins |wc -l`
    if(($status==0));then
        echo "jenkins has been stopped"
        exit 0
    fi
    ps -ef |grep -v grep |grep tomcat-jenkins |awk '{print $2}'|xargs kill -9
    [ $? -eq 0 ]&& action "jenkins is stopping" /bin/true ||\
    action "jenkins stop failure" /bin/false
    ;;
  restart)
    status=`ps -ef |grep -v grep |grep tomcat-jenkins |wc -l`
    if(($status==1));then
                ps -ef |grep -v grep |grep tomcat-jenkins |awk '{print $2}'|xargs kill -9
    fi
    sh  $jenkinspath/bin/startup.sh >& /dev/null
    [ $? -eq 0 ]&& action "jenkins is restarting" /bin/true ||\
    action "jenkins restart failure" /bin/false
    ;;
  *)
    echo "Usage:$0 {start|stop|restart}"
    exit
    ;;
esac

给脚本赋予权限: 
[root@CentOS-jenkins ~]# chmod u+x /etc/init.d/jenkins 
添加到chkconfig列表： 
[root@CentOS-jenkins ~]# chkconfig --add jenkins 
设置开机启动： 
[root@CentOS-jenkins ~]# chkconfig --level 2345 jenkins on























