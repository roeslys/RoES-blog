Linux CentOS 7��װRedis
1����װgcc����
yum install gcc-c++
2.  ����Redisѹ����tar.gz
https://redis.io/
3.  ��Redis�ϴ��� usr/local �£���ѹ��
cd /usr/local
tar -xvf redis-4.0.11.tar.gz
4.  ����redis�ļ��ڣ�����make����
cd redis-4.0.11.tar.gz
make
5.  ��������󣬽��а�װ
make PREFIX=/usr/local/redis install

����redis
1.  ǰ������ģʽ 
ֱ��������װ�õ�redis/bin/redis-server
/usr/local/redis/bin/redis-server
Ĭ�Ͽ�������ǰ��ģʽ
�˿ڣ�6379
2. �������
1.  ��redis��Դ��Ŀ¼���Ƚ�ѹĿ¼���и���redis.conf��redis�İ�װĿ¼/bin�ļ����¡� 
cd usr/local/redis-4.0.11
cp redis.conf ../redis/bin
2.  �޸������ļ�
�޸ģ�daemonizei yes
cd ../redis/bin
vim redis.conf
daemonizei yes
protected-mode yes-->no
bing 127.0.0.0-->0.0.0.0
3.  ����
��redis/bin�������� ./redis-server redis.conf
����ͻ�����֤��./redis-cli
exit�˳��ͻ���
4.  �رպ������
    ./redis-cli shutdown
