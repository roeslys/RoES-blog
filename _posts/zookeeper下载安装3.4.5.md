1�����ص�ַ
�ɰ汾��ַ����
http://archive.apache.org/dist/zookeeper/
�ȶ��汾��ַ
http://mirrors.shu.edu.cn/apache/zookeeper/?C=N;O=A
2����ѹ��
$ tar -zxvf zookeeper-3.4.5.tar.gz
3����zookeeper�Ľ�ѹĿ¼�´���һ�������ļ� conf/zoo.cfg
�������£�

tickTime=2000
dataDir=/home/service/zookeeper-3.4.5/data
dataLogDir=/home/service/zookeeper-3.4.5/logs
clientPort=2181
4����������
./zkServer.sh start

