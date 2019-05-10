1、在Nginx的安装目录下创建cert目录，并且将下载的全部文件拷贝到cert目录中。

2、打开 Nginx 安装目录下 conf 目录中的 nginx.conf 文件，找到：

HTTPS server

#server {

listen 443;

server_name localhost;

ssl on;

ssl_certificate cert.pem;

ssl_certificate_key cert.key;

ssl_session_timeout 5m;

ssl_protocols SSLv2 SSLv3 TLSv1;

ssl_ciphers ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv2:+EXP;

ssl_prefer_server_ciphers on;

location / {

#
#
#}
#}

3、将其修改为 (以下属性中ssl开头的属性与证书配置有直接关系，其它属性请结合自己的实际情况复制或调整) :
server {
 listen 443;
 server_name localhost;
 ssl on;
 root html;
 index index.html index.htm;
 ssl_certificate   /usr/local/nginx/conf/cert/www.baidu.com.pem;
 ssl_certificate_key  /usr/local/nginx/conf/cert/www.baidu.com.key;
 ssl_session_timeout 5m;
 ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
 ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
 ssl_prefer_server_ciphers on;
 location / {
     root html;
     index index.html index.htm;
 }
}

4、设置301跳转，实现http与https看起来像同一个网站
server
    {
        listen 80;
        server_name  www.baidu.com baidu.com;
        index index.html index.htm index.php default.html default.htm default.php;
        root /home/www/baidu;
        return 301 https://www.baidu.com$request_uri;
}

5、启用文件压缩

#-------gzip conf-----
gzip on;
gzip_min_length 1k;
gzip_buffers 4 16k;
#gzip_http_version 1.0;
gzip_comp_level 6;
gzip_types text/plain application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;
gzip_vary off;
gzip_disable "MSIE [1-6]\.";

基本只需要更改gzip_comp_level等级，1-9，等级越高压缩率越高，但相应也越耗CPU资源，一般不会设置可以折中为6.

6、重启nginx服务
./nginx -s reload
systemctl nginx restart