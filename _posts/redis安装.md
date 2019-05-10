Linux CentOS 7安装Redis
1、安装gcc环境
yum install gcc-c++
2.  下载Redis压缩包tar.gz
https://redis.io/
3.  将Redis上传到 usr/local 下，解压缩
cd /usr/local
tar -xvf redis-4.0.11.tar.gz
4.  进入redis文件内，进行make编译
cd redis-4.0.11.tar.gz
make
5.  编译结束后，进行安装
make PREFIX=/usr/local/redis install

启动redis
1.  前端启动模式 
直接启动安装好的redis/bin/redis-server
/usr/local/redis/bin/redis-server
默认开启的是前端模式
端口：6379
2. 后端启动
1.  从redis的源码目录（既解压目录）中复制redis.conf到redis的安装目录/bin文件夹下。 
cd usr/local/redis-4.0.11
cp redis.conf ../redis/bin
2.  修改配置文件
修改：daemonizei yes
cd ../redis/bin
vim redis.conf
daemonizei yes
protected-mode yes-->no
bing 127.0.0.0-->0.0.0.0
3.  启动
在redis/bin下启动： ./redis-server redis.conf
进入客户端验证：./redis-cli
exit退出客户端
4.  关闭后端启动
    ./redis-cli shutdown
