#### 1.  前提是Docker已经安装好了

> 没有安装的可以看这篇文章-->[CentOS 7安装Docker]()



#### 2.拉取Nginx镜像

```shell
docker pull nginx
#使用命令查看拉取到的镜像
docker images
```
![image.png](https://upload-images.jianshu.io/upload_images/7100414-5b458e63101b1829.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



#### 3. 运行Nginx镜像

```shell 
docker run -d -p 80:80 --name nginx nginx
```

参数说明

- ```-d``` 以守护模式运行镜像，也就是后台运行
- ```-p``` 宿主机端口映射的镜像端口，左边是宿主机端口，右边是镜像端口，```80```是Nginx访问端口
- ```--name```给容器起一个唯一的别名

启动后输入```docker ps -a```即可查看运行的容器：

![image.png](https://upload-images.jianshu.io/upload_images/7100414-7afe7820908b9b4f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



#### 4. 访问Nginx

浏览器访问```http://ip```即可,出现以下页面说明运行成功

![image.png](https://upload-images.jianshu.io/upload_images/7100414-9fa5415f42474bb1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 5. 配置Nginx

我们首先需要在宿主机创建用于存放nginx日志、配置文件和相关静态资源的目录

```shell 
mkdir -p /home/service/nginx/log
mkdir -p /home/service/nginx/conf
mkdir -p /home/service/nginx/conf.d
mkdir -p /home/service/nginx/static
mkdir -p /home/service/nginx/ssl
```

然后从Nginx容器中复制一份配置文件到宿主机刚刚创建的conf目录

```shell
docker cp nginx:/etc/nginx/nginx.conf /home/service/nginx/conf/nginx.conf
```

可以看到已经有了

![image.png](https://upload-images.jianshu.io/upload_images/7100414-ca7a9b7741a1e5c6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


查看一下内容

![image.png](https://upload-images.jianshu.io/upload_images/7100414-ab8cf1d95c3aed82.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


上图可以看出，这个配置文件还引入了其他的配置文件，所以我们需要把```include```引入的文件也复制一份到宿主机，但是我们不知道那些文件叫什么，所以我们需要进入容器内查看

```shell
docker exec -it nginx /bin/bash
cd /etc/nginx/conf.d
ls
```

可以看到里面有个default.conf文件

![image.png](https://upload-images.jianshu.io/upload_images/7100414-e884341bfed498c6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们需要把这个文件复制到宿主机，使用```exit```命令退出容器

```shell
exit
docker cp nginx:/etc/nginx/conf.d/default.conf /home/service/nginx/conf.d/default.conf
```

还记得我们前面访问nginx的时候那个页面吗？是的，那个页面也要复制到宿主机

```shell
docker cp nginx:/usr/share/nginx/html/index.html /home/service/nginx/static/index.html
```



#### 6. 修改配置文件

开始修改宿主机上复制出来的conf文件，首先修改```nginx.conf```，修改配置文件修改后的结果：

```nginx
user  nginx;
worker_processes  1;
error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;
worker_rlimit_nofile 65535;

events {
	use epoll;
	worker_connections 65535;
}


http {
    include /etc/nginx/mime.types;
	default_type application/octet-stream;
	charset utf-8;
	keepalive_timeout 60;
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  /var/log/nginx/access.log  main;
    server {
        listen 80;
        server_name  www.roes.top;
	    location / {
	        root   /usr/share/nginx/html;
	        index  index.html index.htm;
	    }
	}
    include /etc/nginx/conf.d/*.conf;
}

```

查看```default.conf```

```nginx
server {
    listen       80;
    server_name  localhost;
    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

}

```

##### 停止上次的nginx容器并删除容器

```shell
docker stop nginx
docker rm nginx
```
##### 重新启动一个nginx镜像

 docker run -p 443:443 -p 80:80 --name nginx \  
    -v /home/service/nginx/static:/usr/share/nginx/html  \  
    -v /home/service/nginx/conf/nginx.conf:/etc/nginx/nginx.conf \    
    -v /home/service/nginx/log:/var/log/nginx  \   
    -v /home/service/nginx/conf.d:/etc/nginx/conf.d \   
    -v /home/service/nginx/ssl:/ssl \    
    -d nginx

```-v```的意思就是把宿主机目录挂载到冒号后面的容器目录

```--link```用于连接容器，后面是零一个容器的唯一name，这样nginx就可以在配置文件使用```jenkins:端口```配置了

**此处多监听了一个443端口，用于以后配置https**

#### 修改配置文件
	修改nginx.conf
	修改index.html
	修改ssl证书等
#### 进入容器重启nginx

```shell
docker exec -it nginx /bin/bash
nginx -s reload
```

配置完成





