本地git172.16.0.117
本地工具服务器 172.16.3.42
管理员账号密码root/Kjz123!@#
/var/local  包
/var/opt/gitlab/backups 备份包

一、安装gitlab
1.1、第一步安装或者配置一些必要环境：

sudo yum install curl openssh-server openssh-server postfix cronie

sudo service postfix start
sudo lokkit -s http -s ssh
sudo chkconfig postfix on

1.2、下载安装gitlab包
centos 7 
https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7
wget https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7/gitlab-ce-11.4.5-ce.0.el7.x86_64.rpm
rpm -Uvh gitlab-ce-11.4.5-ce.0.el7.x86_64.rpm
1.3、修改gitlab配置文件指定服务器Ip和自定义端口
vim  /etc/gitlab/gitlab.rb
external_url 'https://172.16.3.42'
保存并退出，并执行以下命令
sudo gitlab-ctl reconfigure
1.4、浏览器输入gitlab.rb文件中指定的ip
首次登录会提示修改用户名及密码
账号密码：root/bhkj123$%^
安装目录：/etc/gitlab
原git版本信息
[root@localhost /]# cat /opt/gitlab/embedded/service/gitlab-rails/VERSION
10.6.4-ee


二、升级gitlab
给原有gitlab升级到现有git相同版本再做备份，否则无效：
不能跨太多版本所以基本升级到某个大版本的最后一个版本再升级
10.6.4ee--10.6.4ce--10.7.7ce--10.8.7ce--11.2.8ce--11.4.6ce
2.1、下载包
https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7/
2.2、停止相关服务
gitlab-ctl stop unicorn \
gitlab-ctl stop sidekiq \
gitlab-ctl stop nginx 
2.3、升级安装
rpm -Uvh
#rpm -Uvh gitlab-ce-10.0.4-ce.0.el7.x86_64.rpm
2.4、 重新配置gitlab
gitlab-ctl reconfigure
2.5、 重启gitlab
gitlab-ctl restart


三、备份gitlab
3.1、创建备份文件
gitlab-rake gitlab:backup:create
使用以上命令会在/var/opt/gitlab/backups目录下创建压缩备份包
3.2、备份配置文件
/etc/gitlab/gitlab.rb 配置文件须备份
/var/opt/gitlab/nginx/conf nginx配置文件
/etc/postfix/main.cfpostfix 邮件配置备份
四、迁移
4.1、确保新Gitlab服务器和老Gitlab服务器版本相同
4.2、老备份文件目录（/var/opt/gitlab/backups目录）下的备份文件拷贝到新服务器上的/var/opt/gitlab/backups目录
scp root@172.28.17.155:/var/opt/gitlab/backups/1502357536_2017_08_10_9.4.3_gitlab_backup.tar /var/opt/gitlab/backups/
4.3、从备份文件中恢复gitlab
4.3.1、将备份文件权限修改为777
chmod 777 1502357536_2017_08_10_9.4.3_gitlab_backup.tar
4.3.2、执行命令停止相关数据连接服务
gitlab-ctl stop unicorn
gitlab-ctl stop sidekiq
4.3.3、执行命令从备份文件中恢复Gitlab
#gitlab-rake gitlab:backup:restore BACKUP=备份文件编号
gitlab-rake gitlab:backup:restore BACKUP=1502357536_2017_08_10_9.4.3
五、启动git，大功告成
sudo gitlab-ctl start

