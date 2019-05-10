1��Ϊ����conf�����ļ����Ӽ�࣬���������˲�����ҡ�ÿ������һ��conf�����ļ�
include             /data/nginx/conf/vhost/*.conf;
2���ڣ�ÿ����������ǰ��ӱ�ע�����ָ���������#ע�͵�����
#-------------www.baidu.com.conf----------------------------
3������301��ת��ʹ��http����https����������������һ����վ
server
    {
        listen 80;
        server_name  www.buhuokeji.com buhuokeji.com;
        index index.html index.htm index.php default.html default.htm default.php;
        root /home/www/buhuokeji.com;
        return 301 https://www.baidu.com$request_uri;
}
4�����404��ҳ
server
{
listen 80;
server_name www.baidu.com; #������
error_page 404 /404.html;
} 
5������nginx����������
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

systemctl enable nginx.service ���ÿ�������

killall nginx
systemctl start nginx.service    ����nginx
systemctl stop nginx.service    ����nginx
systemctl restart nginx.service    ����nginx

6������������������������
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

����zentao����
systemctl start zentao.service
���ÿ�����������
systemctl enable zentao.service
ֹͣ������������
systemctl disable zentao.service
�鿴����ǰ״̬��
systemctl status zentao.service
������������
systemctl restart zentao.service
�鿴�����������ķ���
systemctl list-units --type=service

7������redis��������
vi /usr/local/redis/redis.conf
����doemonize yes
����redis.conf��/etc/redis/6379.conf
mkdir /etc/redis
cp /usr/local/redis/redis.conf /etc/redis/6379
����redis�����ű���/etc/init.d/redis
find / -name redis_init_script
cp /usr/redis/redis-3.2.4//utils/redis_init_script /etc/init.d/redis
�޸���������
vi /etc/init.d/redis
��/bin/bash�����ע��
# chkconfig: 2345 10 90  
# description: Start and Stop redis

REDISPORT=6379
EXEC=/usr/local/redis/bin/redis-server
CLIEXEC=/usr/local/redis/bin/redis-cli

PIDFILE=/var/run/redis_${REDISPORT}.pid
CONF="/etc/redis/${REDISPORT}.conf"

����redis
��redis����:service redis start
�ر�redis����:service redis stop
��Ϊ��������:chkconfig redis on
��Ϊ�����ر�:chkconfig redis off



8��jenkins���������ű�
vim /etc/init.d/jenkins
#! /bin/bash
#chkconfig:345 80 80      
#345�ǰ�ȫ����,80 80 ��������Ӧ���ȼ� 
#description:Jenkins
#��������ʱ����

#Jenkins�Ĵ��λ�ã��ļ���������tomcat-jenkins(������õ�)
jenkinspath=/data/tomcat-jenkins/

#���غ�����
. /etc/init.d/functions
#�ж�·���Ƿ����
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

���ű�����Ȩ��: 
[root@CentOS-jenkins ~]# chmod u+x /etc/init.d/jenkins 
��ӵ�chkconfig�б� 
[root@CentOS-jenkins ~]# chkconfig --add jenkins 
���ÿ��������� 
[root@CentOS-jenkins ~]# chkconfig --level 2345 jenkins on























