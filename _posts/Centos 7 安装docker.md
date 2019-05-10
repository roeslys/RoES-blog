Centos 7 安装 docker

## <center>CentOS 7 安装Docker</center>

### 1. 安装Docker的前提条件
> - CentOS 7 64位：系统内核3.10以上
> - CentOS 6.5 要求为64位、系统内核版本为 2.6.32-431以上
> - 查看内核版本命令：```uname -r```

### 2. yum安装

``` shell
yum update -y
#sudo yum install -y yum-utils device-mapper-persistent-data lvm2
#sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum -y install docker
yum install docker-ce
systemctl start docker
```



#### 3. 检查启动是否成功

```shell
docker version
```

出现如下输出即正常：

> Client:
>  Version:         1.13.1
>  API version:     1.26
>  Package version: docker-1.13.1-75.git8633870.el7.centos.x86_64
>  Go version:      go1.9.4
>  Git commit:      8633870/1.13.1
>  Built:           Fri Sep 28 19:45:08 2018
>  OS/Arch:         linux/amd64
>
> Server:
>  Version:         1.13.1
>  API version:     1.26 (minimum version 1.12)
>  Package version: docker-1.13.1-75.git8633870.el7.centos.x86_64
>  Go version:      go1.9.4
>  Git commit:      8633870/1.13.1
>  Built:           Fri Sep 28 19:45:08 2018
>  OS/Arch:         linux/amd64
>  Experimental:    false

#### 4. 更改安装目录

centos 7 默认安装路径是 /var/lib/docker ，而这个是根目录，一般不会太大，修改存储目录到 /home/data/docker

```
systemctl stop docker
mkdir /home/data/docker
mv /var/lib/docker/* /home/data/docker
cd /var/lib
rm -rf docker
进入/home/data/docker 目录建立软连接
ln -s /home/data/docker/ /var/lib/docker
进入 /var/lib 查看 
ls -la docker
```

#### 4. 配置国内镜像仓库源

docker默认连接的镜像仓库是国外的，速度不是很快，所以我们可以配置国内的docker镜像仓库。

```shell
vim /etc/docker/daemon.json
```

输入以下配置即可：

```shell
{
  "registry-mirrors": ["https://registry.docker-cn.com"]
}
```

保存、重启。

```shell
systemctl restart docker
```

