1、查看是否安装和运行cron
ps -ef | grep cron
rpm -qa | grep crontab
2、服务命令
service crond start    //启动服务
service crond stop     //关闭服务
service crond restart  //重启服务
service crond reload   //重新载入配置
service crond status   //查看服务状态
3、全局配置文件
/etc
存在cron.hourly,cron.daily,cron.weekly,cron.monthly,cron.d五个目录和crontab,cron.deny二个文件。
cron.daily是每天执行一次的job
cron.weekly是每个星期执行一次的job
cron.monthly是每月执行一次的job
cron.hourly是每个小时执行一次的job
cron.d是系统自动定期需要做的任务
crontab是设定定时任务执行文件
cron.deny文件就是用于控制不让哪些用户使用Crontab的功能
4、用户配置文件
crontab -e 存放目录 >> /var/spool/cron/
5、crontab格式
*           *        *        *        *           command

minute   hour    day   month   week      command

分          时         天      月        星期       命令

15 8 * * * /home/backup.sh 每天8点15执行一次脚本