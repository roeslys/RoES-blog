
####查看主机IP和路由
route -n
ifconfig
查看容器IP
docker inspect 容器ID或名称 

####docker 常用命令
启动
systemctl start docker
停止
systemctl stop docker
查看docker是否启动
systemctl status docker
查看镜像
docker images
查看运行容器
docker ps
查看所有容器
docker ps -a
启动一个容器
docker run -d -v jenkins_home:/home/service/jenkins -p 7070:8080 -p 50000:50000 --name jenkins jenkins/jenkins:lts
参数说明

- ```-d``` 以守护模式运行镜像，也就是后台运行
- ```-v ``` 把Jenkins容器内的目录挂载到宿主机的目录
- ```-p``` 宿主机端口映射的镜像端口，左边是宿主机端口，右边是镜像端口，```7070```是Jenkins访问端口，另外还要暴露一个```tcp```端口```50000```
- ```--name```给容器起一个唯一的别名
删除一个未运行的容器
docker rm 容器ID或名称
删除一个正在运行的容器
docker rm -f 容器ID或名称
删除一个镜像
docker rmi <image id>  #删除镜像前需要先停止运行容器和删除容器然后删除镜像
进入一个容器
docker exec -it jenkins /bin/bash  #jenkins为--name的容器名
容器通信
--link 容器名
宿主机目录挂在到容器目录
- ```-v ``` 把Jenkins容器内的目录挂载到宿主机的目录
```shell
docker run -p 80:80  -p 443:443 --name nginx \
    --link jenkins  \
    -v /home/service/nginx/static:/usr/share/nginx/html \
    -v /home/service/nginx/log:/var/log/nginx \
    -v /home/service/nginx/conf/nginx.conf:/etc/nginx/conf \
    -d nginx
```
启动一个已经停止的容器
```
docker start 容器名
```