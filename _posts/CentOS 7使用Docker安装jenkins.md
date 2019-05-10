## <center>CentOS 7 使用Docker搭建Jenkins</center>

#### 1.  前提是Docker已经安装好了

> 没有安装的可以看这篇文章-->[CentOS 7安装Docker]()



#### 2.拉取Jenkins镜像

```shell
docker pull jenkins/jenkins:lts
#使用命令查看拉取到的镜像
docker images
```
#### 3. 运行JenKins镜像

```shell 
mkdir -p /home/service/jenkins
chown -R 1000 /home/service/jenkins
docker run -d -v /home/service/jenkins:/var/jenkins_home -p 7070:8080 -p 50000:50000 --name jenkins jenkins/jenkins:lts
docker run --name jenkins -p 8080:8080 -p 50000:50000 -v /var/run/docker.sock:/var/run/docker.sock -v /home/service/jenkins:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -d jenkins/jenkins:lts
```

参数说明

- ```-d``` 以守护模式运行镜像，也就是后台运行
- ```-v ``` 把Jenkins容器内的目录挂载到宿主机的目录
- ```-p``` 宿主机端口映射的镜像端口，左边是宿主机端口，右边是镜像端口，```7070```是Jenkins访问端口，另外还要暴露一个```tcp```端口```50000```
- ```--name```给容器起一个唯一的别名

启动后输入```docker ps -a```即可查看运行的容器：

#### 4. 访问Jenkins

浏览器访问```http://ip:7070```即可进入Jenkins

但是我希望通过nginx转发进入jenkins，所以接下来配置nginx转发到Jenkins

没有安装nginx的可以看这里[CentOS 7使用Docker搭建Nginx]()

#### 5.实现同主机不同容器通信

刚开始配的时候，傻傻的直接用nginx转发到7070端口，结果发现怎么都访问不到，琢磨半天之后突然想起来前几天看的【Docker技术入门与实战第二版】中说的容器通信，让我恍然大悟

容器通信方式很多种，我这里选择docker的link机制来访问

停止并删除nginx容器

```shell
docker stop nginx
docker rm nginx
```

首先修改nginx的配置文件：

```nginx
	location / {
		#代理地址:将请求转到该地址,jenkins为容器名
		proxy_pass http://jenkins:7070;
		proxy_connect_timeout 600;
		proxy_read_timeout 600;
	}
```
在nginx容器启动命令使用--link 容器名 来访问Jenkins容器

docker run -p 81:80 --name nginx --link jenkins  -v /home/service/nginx/static:/usr/share/nginx/html -v /home/service/nginx/log:/var/log/nginx -v /home/service/nginx/conf/nginx.conf:/etc/nginx/conf -d nginx

```shell
docker run -p 80:80  -p 443:443 --name nginx \
    --link jenkins  \
    -v /home/service/nginx/static:/usr/share/nginx/html \
    -v /home/service/nginx/log:/var/log/nginx \
    -v /home/service/nginx/conf/nginx.conf:/etc/nginx/conf \
    -d nginx
```

然后访问```https://ip```访问即可,第一次进入需要登陆

密码需要进入jenkins容器里面获取

```shell	
docker exec -it jenkins /bin/bash
cat /var/jenkins_home/secrets/initialAdminPassword
```

得到的密码登陆即可


#### 6. 配置Jenkins

想方便的可以直接安装推荐的插件，我选择自定义安装



之后慢慢等待安装完成即可，时间有点久

#### 6.给jenkins配置一个二级域名

