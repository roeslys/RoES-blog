Jenkins、rap设置开机自启动

将全路径下的启动脚本添加到/etc/rc.local 里。

```shell
[root@bogon bin]# pwd
/home/service/apache-tomcat-rapapi/bin
[root@bogon bin]# ls
bootstrap.jar  catalina.sh         commons-daemon.jar            configtest.bat  daemon.sh   digest.sh  setclasspath.bat  shutdown.bat  startup.bat  tomcat-juli.jar       tool-wrapper.bat  version.bat
catalina.bat   catalina-tasks.xml  commons-daemon-native.tar.gz  configtest.sh   digest.bat  logs       setclasspath.sh   shutdown.sh   `startup.sh`  tomcat-native.tar.gz  tool-wrapper.sh   version.sh
```

```shell
[root@bogon bin]# pwd
/home/service/apache-tomcat-jenkins/bin
[root@bogon bin]# ls
bootstrap.jar  catalina.sh         commons-daemon.jar            configtest.bat  daemon.sh   digest.sh  setclasspath.bat  shutdown.bat  startup.bat  tomcat-juli.jar       tool-wrapper.bat  version.bat
catalina.bat   catalina-tasks.xml  commons-daemon-native.tar.gz  configtest.sh   digest.bat  logs       setclasspath.sh   shutdown.sh   `startup.sh`   tomcat-native.tar.gz  tool-wrapper.sh   version.sh
```



```shell

#!/bin/bash
# THIS FILE IS ADDED FOR COMPATIBILITY PURPOSES
#
# It is highly advisable to create own systemd services or udev rules
# to run scripts during boot instead of using this file.
#
# In contrast to previous versions due to parallel execution during boot
# this script will NOT be run after all other services.
#
# Please note that you must run 'chmod +x /etc/rc.d/rc.local' to ensure
# that this script will be executed during boot.

touch /var/lock/subsys/local
/home/service/apache-tomcat-jenkins/bin/startup.sh
/home/service/apache-tomcat-rapapi/bin/startup.sh

```

