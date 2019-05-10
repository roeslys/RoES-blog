1、查找Docker Hub上的php镜像
docker search php
2、拉取官方的镜像
docker pull php:5.6-fpm
3、本地镜像列表里查看
docker images
4、宿主机创建目录用于容器文件挂载
mkdir -p /home/service/php/conf
mkdir -p /home/service/php/logs
5、运行php镜像
docker run -p 9000:9000 --name  php-fpm \
-v /home/service/php/conf:/usr/local/etc/php \
-v /home/service/php/logs:/phplogs \ 
-d php:5.6-fpm
6、查看容器启动情况
docker ps
7、通过浏览器访问验证phpinfo
访问路径直接 http://ip/index.php


