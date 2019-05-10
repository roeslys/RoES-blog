## <center>CentOS 7使用Docker安装Redis</center>

#### 1.  前提是Docker已经安装好了

> 没有安装的可以看这篇文章-->[CentOS 7安装Docker]()



#### 2.拉取Redis镜像

```shell
docker pull redis
```



#### 3.启动

```shell
docker run --name redis -p 6378:6379 -d --restart=always redis redis-server --appendonly yes --requirepass "bhkj2019"
```

> 参数讲解
>
> ```--name```为容器取一个唯一的名字
>
> ```-p```端口映射，把宿主机的端口映射到容器内的端口
>
> ```--restar=always```随容器启动而启动
>
> ```redis-server --appendonly yes```在容器里执行```redis-server```命令，打开redis持久化
>
> ```--requirepass```密码



#### 4.连接

```
docker ps 查看CONTAINER ID
docker exec -it 0047b8680fc2 redis-cli
```

