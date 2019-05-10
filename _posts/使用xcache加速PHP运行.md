# [使用xcache加速PHP运行](<https://www.jianshu.com/u/22202ba94f03>)



 XCache 是一个开源的 opcode 缓存器/优化器, 这意味着他能够提高您服务器上的 PHP 性能. 他通过把编译 PHP 后的数据缓冲到共享内存从而避免重复的编译过程, 能够直接使用缓冲区已编译的代码从而提高速度. 通常能够提高您的页面生成速率 2 到5 倍, 降低服务器负载.

　　目前用于Web的缓存系统很多，包括squid、varnish、Nginx自带的proxy_cache、FastCGI中的fastcgi_cache、APC、Xcache等。
　　像squid、varnish、Nginx自带的proxy_cache这类系统，属于重量级产品，配置维护比较麻烦，不适合小型网站，而且一般用这类系统缓存静态内容，比如图片、css、JavaScript等；像FastCGI中的fastcgi_cache，它主要用于缓存动态内容，所以在访问使用fastcgi_cache的网站时速度极快，但是笔者使用时发现其维护比较麻烦，特别是每次网站有数据要更新后，如果不等到缓冲期过期后得需要手动清除缓存才能看到网站更新的内容；至于APC个人感觉性能就一般了，拿它和Xcache比较时发现访问使用Xcache网站的速度明显高于使用APC网站的速度（笔者没有具体测试），所以最终选择了使用Xcache。
　　我们都知道PHP是一种动态语言，它在执行时是以解释的方式执行，所以PHP代码每次执行时都会被解析和转换成操作码（opcode）。而Xcache是一个开源的操作码缓存器/优化器，它通过把解析/转换PHP后的操作码缓存到文件（直到原始代码被修改）从而避免重复的解析过程，提高了代码的执行速度，通常能够提高页面生成速率2-5倍，降低了服务器负载，提高了用户访问网站的速度。

一、安装Xcache
```
# wget http://xcache.lighttpd.net/pub/Releases/1.3.0/xcache-1.3.0.tar.gz

# tar zxvf xcache-1.3.0.tar.gz

# cd xcache-1.3.0

# /usr/local/php/bin/phpize

# ./configure --enable-xcache--enable-xcache-coverager --enable-xcache-optimizer--with-php-config=/usr/local/php/bin/php-config

# make && make install
```

　　注：--enable-xcache表示启用Xcache支持；--enable-xcache-coverager表示包含用于测量加速器功效的附加特性；--enable-xcache-optimizer表示启用操作码优化
　　安装完毕后系统会提示xcache.so模块生成路径，本次生成路径为/usr/local/php/lib/php/extensions/no-debug-non-zts-20060613/，然后把xcache.so移动到/usr/local/php/include/php/ext目录下。

二、配置管理Xcache
1、修改php配置文件
　　配置时我们可以参考xcache的配置模板xcache.ini，此文件位于Xcache安装程序中
```
vi /usr/local/php/lib/php.ini
```

　　然后添加如下内容

```
extension_dir=/usr/local/php/include/php/ext
[xcache-common]
extension = xcache.so
[xcache.admin]
xcache.admin.enable_auth = On
xcache.admin.user = "xcache"
xcache.admin.pass = ""
[xcache]
xcache.shm_scheme ="mmap"
xcache.size=60M
xcache.count =1
xcache.slots =8K
xcache.ttl=0
xcache.gc_interval =0
xcache.var_size=4M
xcache.var_count =1
xcache.var_slots =8K
xcache.var_ttl=0
xcache.var_maxttl=0
xcache.var_gc_interval =300
xcache.test =Off
xcache.readonly_protection = On
xcache.mmap_path ="/tmp/xcache"
xcache.coredump_directory =""
xcache.cacher =On
xcache.stat=On
xcache.optimizer =Off
[xcache.coverager]
xcache.coverager =On
xcache.coveragedump_directory =""
```

2、生成Xcache缓存文件

2、生成Xcache缓存文件

```
# touch /tmp/xcache

# chmod 777 /tmp/xcache
```

3、生成Xcache管理员的秘密（MD5密文）
```
echo -n "123456" | md5sum
```

e10adc3949ba59abbe56e057f20f883e
　　然后将上述生成的MD5密文粘贴到php.ini文件中xcache.admin.pass = ""选项，xcache.admin.pass= "e10adc3949ba59abbe56e057f20f883e"
4、拷贝Xcache管理程序到网站根目录下

```
cp -a /tmp/xcache-1.3.0/admin//usr/local/nginx/html/
```

　　然后重新启动PHP，然后访问http://localhost/admin ，用户名为xcache 密码为123456；另外，还可以通过phpinfo来验证PHP是否支持Xcache