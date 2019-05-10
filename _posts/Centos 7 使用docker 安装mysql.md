

## <center>CentOS 7 使用Docker搭建MySQL</center>

#### 1.  前提是Docker已经安装好了

> 没有安装的可以看这篇文章-->[CentOS 7安装Docker]()



#### 2.拉取mysql镜像

```shell
docker pull mysql
docker images
```
可以看到我们拉取的镜像
![1543688678741](C:\Users\BH-JiLin\AppData\Roaming\Typora\typora-user-images\1543688678741.png)

宿主机创建几个文件夹用于容器文件挂载

```shell
mkdir -p /home/service/mysql/data
mkdir -p /home/service/mysql/log
mkdir -p /home/service/mysql/conf
```

然后运行命令启动mysql

```
docker run -p 3306:3306 --name mysql \
  -v /home/service/mysql/logs:/logs \
  -v /home/service/mysql/data:/mysql_data \
  -e MYSQL_ROOT_PASSWORD=bhkj2019! \
  -d mysql
```

docker run -p 3307:3306 --name mysql -v /home/service/mysql/logs:/logs -v /home/service/mysql/data:/mysql_data -e MYSQL_ROOT_PASSWORD=bhkj2019! -d mysql

> 命令讲解
> ```-p``` 3306:3306：将容器的3306端口映射到主机的3306端口
> ```-v``` 将主机~/mysql/logs目录挂载到容器的/logs
> ```-v``` 将主机mysql/data目录挂载到容器的/mysql_data
> ```-e``` MYSQL_ROOT_PASSWORD=zhoujilin：初始化root用户的密码

进入容器

```shell
docker exec -it mysql bash
```

登陆mysql

```shell
mysql -uroot -p
```

![1543690331735](C:\Users\BH-JiLin\AppData\Roaming\Typora\typora-user-images\1543690331735.png)

创建mysql用户

```mysql
CREATE USER 'admin'@'%' IDENTIFIED BY 'bhkj2019!';
GRANT ALL ON *.* TO 'admin'@'%';
flush privileges;
```

然后就可以使用admin用户登陆了