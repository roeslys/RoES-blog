1���鿴�Ƿ�װ������cron
ps -ef | grep cron
rpm -qa | grep crontab
2����������
service crond start    //��������
service crond stop     //�رշ���
service crond restart  //��������
service crond reload   //������������
service crond status   //�鿴����״̬
3��ȫ�������ļ�
/etc
����cron.hourly,cron.daily,cron.weekly,cron.monthly,cron.d���Ŀ¼��crontab,cron.deny�����ļ���
cron.daily��ÿ��ִ��һ�ε�job
cron.weekly��ÿ������ִ��һ�ε�job
cron.monthly��ÿ��ִ��һ�ε�job
cron.hourly��ÿ��Сʱִ��һ�ε�job
cron.d��ϵͳ�Զ�������Ҫ��������
crontab���趨��ʱ����ִ���ļ�
cron.deny�ļ��������ڿ��Ʋ�����Щ�û�ʹ��Crontab�Ĺ���
4���û������ļ�
crontab -e ���Ŀ¼ >> /var/spool/cron/
5��crontab��ʽ
*           *        *        *        *           command

minute   hour    day   month   week      command

��          ʱ         ��      ��        ����       ����

15 8 * * * /home/backup.sh ÿ��8��15ִ��һ�νű�