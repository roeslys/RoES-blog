1、查找Docker Hub上的tomcat镜像
docker search tomcat
2、拉取官方镜像
docker pull tomcat
3、查看镜像是否存在
docker images
4、宿主机创建相关目录挂载到容器主机上
mkdir -p /home/service/tomcat/conf
mkdir -p /home/service/tomcat/webapps/test
mkdir -p /home/service/tomcat/logs
5、创建并运行容器
docker run --name tomcat -p 8080:8080 \
-v /home/service/tomcat/webapps/test:/usr/local/tomcat/webapps/test \
-d tomcat
6、通过浏览器访问
地址：http://ip:8080