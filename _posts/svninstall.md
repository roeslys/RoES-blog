
##一、备份VisualSVN项目
1. 现在要使用Linux作为svn服务器，之前是在windows Server 2008上的，用的是VisualSVN，作者除了迁移了svn还有禅道，gitlab等，为此可是查阅了很多资料，为此做一个总结，希望以后遇到类似问题的有资料可查，有兴趣的可以看看我的其他随笔。
2. 现在svn中有一个项目fpp，需要将fpp这个项目导出到linux环境下。运行cmd，输入命令 svnadmin dump E:\Repositories\fpp > e:\svnbak\fpp.dump将项目导出到e:\svnbak目录下。
3. 可见命令会导出每个版本的详细信息，保证了之前的历史信息不会丢失。现在我们便得到备份文件fpp.dump.
##二、上传备份文件到Linux
1. 利用ftp, ssh，scp等工具将fpp.dump文件传输到Linux服务器上，这里利用直接利用openSSH这个软件上传。文件的路径为/root/fpp.dump。
##三、Linux下SVN的安装与配置
1. Linux下安装svn，这里直接利用yum命令进行安装。`yum install subversion`完成subversion的安装。
2. 建立版本库目录svndata。
`mkdir /svndata`
`svnserve -d -r /svndata` #启动svn，设置版本库目录为/svndata
> 有时启动失败的话，查看端口是否被占用，`kill -9 1524`(进程号)，杀死进程再运行命令
` svnserve -d -r /svndata `
不知道是那个进程也可以直接杀死所有进程
`killall svnserve` #关闭svn
3. 建立项目库
`svnadmin create /svndata/fpp`  #fpp就是你的项目名，这个以后要用到
4. 配置用户访问权限
`cd /svndata/fpp/conf`
`vi svnserve.conf`
> 释放如下几行的注释 ,之前查资料遇到一些什么都不懂就敢往上发，还有的人说把这几个注释了，有的是第一项参数为read的话就不用设置账号了。
anon-access = none    #匿名用户不可读写，人多需要设置权限必须设置为none
auth-access = write    #授权用户可写
password-db = passwd  #以哪个文件作为用户密码文件
authz-db = authz    #以哪个文件作为权限文件
后面两个也可以写绝对路径，建议写绝对路径。
password-db = /svndata/fpp/conf/passwd
authz-db =  /svndata/fpp/conf/authz
5. 增加访问用户，格式为（username = password）
> 特别注意！！！等号两边要加空格，否则无效。没有加空格，就一直没用，在linux所有配置文件里都是注意
##四、导入备份文件
1. 输入命令： ` svnadmin load /svndata/fpp < /root/fpp.dump `
##五、客户端进行代码的检出
1.windows端安装TortoiseSVN, 右键svn checkout
2. 在打开的对话框中，输入svn库的地址，确定便可以同步项目，ip地址加项目名称。
3. svn提示检出成功，在目录下可以找到检出的项目。
4. 对于以前的项目，重定向到新的svn服务器，右键->TortoiseSVN->Relocate，在弹出的对话框中填写新的地址，TortoiseSVN会提示修改成功，之后，就可以使用新的svn了。

常见问题总结如下：
1、不知道该怎么设置 svn://url 这个路径
2、三个需要设置的文件，其中authz这个里面的[repos:/]这个到底该怎么设置
3、认证失败问题出在哪里？
4、svn import 目录1 "svn://localhost/目录2" -m "first version" 目录2到底怎么设置？
5、import 的时候出现“条目从本地编码转换到UTF8失败”
6、服务器端都没问题了，但是客户端不能连接主机
下面就根据这几个问题，一一解答：

1、svn可以分为单个或多个版本库，假设：
版本库目录为 /data/svndata/repos1
启动程序如果是：svnserve -d -r /data/svndata/repos1  
这代表你当前svn只为repos1这个版本库工作，客户端访问直接svn://IP/ 就可以了，后面不跟目录
启动程序如果是：svnserve -d -r /data/svndata/          这代表你当前svn可以多版本库运行，客户端访问就需要加上 svn://IP/repos1 这样才能访问repos1版本库
2、第一个问题是对应的
如果是一个版本库，那应该设置成如下：
[groups]
admin = user1,user2
[/]
@admin=rw
如果是多个版本库，那就应该设置成这样：
[groups]
admin = user1,user2
[repos1:/]
@admin = rw
3、认证失败的问题，就是对上述两个问题没有相对应的设置好，要么都安一个版本库设置，要么都安多个版本库设置，只要对应设置好，应该就是没有问题的。
4、目录2是由svn建立的，不用自己去设置，假设：
svn import /tmp/ceshi "svn://localhost/a/b/c" -m "first version"
这样的话，当你checkout的时候，你本地的目录就应该是: /a/b/c
5、网上都说是LANG没设置好，可是我的不是这个问题，我的是导入的源文件中有些文件自身的文件名乱码，建议使用sublime而不要用notepad。
6、服务器都设置好了，那要是客户端还连不上，就是防火墙的问题了，去/etc/sysconfig/iptables 设置一下，打开默认的3690端口就可以了

参考原文地址：https://www.cnblogs.com/lidabo/p/4633152.html
https://www.cnblogs.com/lxwphp/p/8031811.html



