1、安装lrzsz
yum install -y lrzsz

2、sz命令发送文件到本地
# sz filename
输入命令后会弹出接受文件选择目录
[root@jubeiTest data0]# sz nohup.out      

3、rz命令本地上传文件到服务器：
# rz
执行该命令后，在弹出框中选择要上传的文件即可。上传到当前目录下，如何你要上传到/home/www下就直接先切换带目下
cd /home/www
rz 