1����Nginx�İ�װĿ¼�´���certĿ¼�����ҽ����ص�ȫ���ļ�������certĿ¼�С�

2���� Nginx ��װĿ¼�� conf Ŀ¼�е� nginx.conf �ļ����ҵ���

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

3�������޸�Ϊ (����������ssl��ͷ��������֤��������ֱ�ӹ�ϵ���������������Լ���ʵ��������ƻ����) :
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

4������301��ת��ʵ��http��https��������ͬһ����վ
server
    {
        listen 80;
        server_name  www.baidu.com baidu.com;
        index index.html index.htm index.php default.html default.htm default.php;
        root /home/www/baidu;
        return 301 https://www.baidu.com$request_uri;
}

5�������ļ�ѹ��

#-------gzip conf-----
gzip on;
gzip_min_length 1k;
gzip_buffers 4 16k;
#gzip_http_version 1.0;
gzip_comp_level 6;
gzip_types text/plain application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;
gzip_vary off;
gzip_disable "MSIE [1-6]\.";

����ֻ��Ҫ����gzip_comp_level�ȼ���1-9���ȼ�Խ��ѹ����Խ�ߣ�����ӦҲԽ��CPU��Դ��һ�㲻�����ÿ�������Ϊ6.

6������nginx����
./nginx -s reload
systemctl nginx restart