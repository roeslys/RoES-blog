Centos 7 ��װ docker

## <center>CentOS 7 ��װDocker</center>

### 1. ��װDocker��ǰ������
> - CentOS 7 64λ��ϵͳ�ں�3.10����
> - CentOS 6.5 Ҫ��Ϊ64λ��ϵͳ�ں˰汾Ϊ 2.6.32-431����
> - �鿴�ں˰汾���```uname -r```

### 2. yum��װ

``` shell
yum update -y
#sudo yum install -y yum-utils device-mapper-persistent-data lvm2
#sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum -y install docker
yum install docker-ce
systemctl start docker
```



#### 3. ��������Ƿ�ɹ�

```shell
docker version
```

�������������������

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

#### 4. ���İ�װĿ¼

centos 7 Ĭ�ϰ�װ·���� /var/lib/docker ��������Ǹ�Ŀ¼��һ�㲻��̫���޸Ĵ洢Ŀ¼�� /home/data/docker

```
systemctl stop docker
mkdir /home/data/docker
mv /var/lib/docker/* /home/data/docker
cd /var/lib
rm -rf docker
����/home/data/docker Ŀ¼����������
ln -s /home/data/docker/ /var/lib/docker
���� /var/lib �鿴 
ls -la docker
```

#### 4. ���ù��ھ���ֿ�Դ

dockerĬ�����ӵľ���ֿ��ǹ���ģ��ٶȲ��Ǻܿ죬�������ǿ������ù��ڵ�docker����ֿ⡣

```shell
vim /etc/docker/daemon.json
```

�����������ü��ɣ�

```shell
{
  "registry-mirrors": ["https://registry.docker-cn.com"]
}
```

���桢������

```shell
systemctl restart docker
```

