1、下载apache-tomcat
http://tomcat.apache.org/download-80.cgi
2、解压
tar -zxvf apache-tomcat-8.5.35.tar.gz
[root@izysl2jv0d6fukz root]# tar -zxvf apache-tomcat-8.5.35.tar.gz
apache-tomcat-8.5.35/conf/
apache-tomcat-8.5.35/conf/catalina.policy
apache-tomcat-8.5.35/conf/catalina.properties
apache-tomcat-8.5.35/conf/context.xml
apache-tomcat-8.5.35/conf/jaspic-providers.xml
apache-tomcat-8.5.35/conf/jaspic-providers.xsd
apache-tomcat-8.5.35/conf/logging.properties
apache-tomcat-8.5.35/conf/server.xml
apache-tomcat-8.5.35/conf/tomcat-users.xml
apache-tomcat-8.5.35/conf/tomcat-users.xsd
apache-tomcat-8.5.35/conf/web.xml
apache-tomcat-8.5.35/bin/
apache-tomcat-8.5.35/lib/
apache-tomcat-8.5.35/logs/
3、jinkins war 包 放在webapps下

4、直接解压 /bin下运行 ./startup.sh
[root@izysl2jv0d6fukz bin]# ls
bootstrap.jar  catalina.sh         commons-daemon.jar            configtest.bat  daemon.sh   digest.sh         setclasspath.sh  shutdown.sh  startup.sh       tomcat-native.tar.gz  tool-wrapper.sh  version.sh
catalina.bat   catalina-tasks.xml  commons-daemon-native.tar.gz  configtest.sh   digest.bat  setclasspath.bat  shutdown.bat     startup.bat  tomcat-juli.jar  tool-wrapper.bat      version.bat
[root@izysl2jv0d6fukz bin]# ./startup.sh
Using CATALINA_BASE:   /root/apache-tomcat-8.5.35
Using CATALINA_HOME:   /root/apache-tomcat-8.5.35
Using CATALINA_TMPDIR: /root/apache-tomcat-8.5.35/temp
Using JRE_HOME:        /usr
Using CLASSPATH:       /root/apache-tomcat-8.5.35/bin/bootstrap.jar:/root/apache-tomcat-8.5.35/bin/tomcat-juli.jar
Tomcat started.

