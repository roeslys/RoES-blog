rpm ��װjdk
1������
http://www.oracle.com/technetwork/java/javase/downloads/index.html
2��Ȩ��
[root@izysl2jv0d6fukz root]# chmod 755 jdk-8u191-linux-x64.rpm
[root@izysl2jv0d6fukz root]# ll
total 429312
drwxr-xr-x 9 root root      4096 Dec  6 19:26 apache-tomcat-8.5.35
-rw-r--r-- 1 root root   9642757 Nov 26 17:23 apache-tomcat-8.5.35.tar.gz
-rw-rw-r-- 1 root mail       135 Dec  6 13:53 dead.letter
-rwxr-xr-x 1 root root 176154027 Oct 25 09:28 jdk-8u191-linux-x64.rpm
drwxr-xr-x 7 root root      4096 Dec  5 15:46 lnmp
drwxr-xr-x 7 root root      4096 Nov 13 10:04 lnmp1.5
-rw-r--r-- 1 root root    149732 Nov 13 10:44 lnmp1.5.tar.gz
-rw-r--r-- 1 root root 252753704 Dec  5 09:01 lnmp-full.tar.gz
-rw-r--r-- 1 root root    881425 Dec  4 10:07 lnmp-install.log
3����װ
[root@izysl2jv0d6fukz root]# rpm -i jdk-8u191-linux-x64.rpm
warning: jdk-8u191-linux-x64.rpm: Header V3 RSA/SHA256 Signature, key ID ec551f03: NOKEY
Unpacking JAR files...
	tools.jar...
	plugin.jar...
	javaws.jar...
	deploy.jar...
	rt.jar...
	jsse.jar...
	charsets.jar...
	localedata.jar...
4���鿴 java -version
[root@izysl2jv0d6fukz root]# java -version
java version "1.8.0_191"
Java(TM) SE Runtime Environment (build 1.8.0_191-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.191-b12, mixed mode)
5�����û�������
[root@izysl2jv0d6fukz root]# vi /etc/profile  ����������������ݣ�
export JAVA_HOME=/root/java/jdk1.8.0_191
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar 
export PATH=$PATH:$JAVA_HOME/bin