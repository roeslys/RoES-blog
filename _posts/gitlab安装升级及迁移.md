����git172.16.0.117
���ع��߷����� 172.16.3.42
����Ա�˺�����root/Kjz123!@#
/var/local  ��
/var/opt/gitlab/backups ���ݰ�

һ����װgitlab
1.1����һ����װ��������һЩ��Ҫ������

sudo yum install curl openssh-server openssh-server postfix cronie

sudo service postfix start
sudo lokkit -s http -s ssh
sudo chkconfig postfix on

1.2�����ذ�װgitlab��
centos 7 
https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7
wget https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7/gitlab-ce-11.4.5-ce.0.el7.x86_64.rpm
rpm -Uvh gitlab-ce-11.4.5-ce.0.el7.x86_64.rpm
1.3���޸�gitlab�����ļ�ָ��������Ip���Զ���˿�
vim  /etc/gitlab/gitlab.rb
external_url 'https://172.16.3.42'
���沢�˳�����ִ����������
sudo gitlab-ctl reconfigure
1.4�����������gitlab.rb�ļ���ָ����ip
�״ε�¼����ʾ�޸��û���������
�˺����룺root/bhkj123$%^
��װĿ¼��/etc/gitlab
ԭgit�汾��Ϣ
[root@localhost /]# cat /opt/gitlab/embedded/service/gitlab-rails/VERSION
10.6.4-ee


��������gitlab
��ԭ��gitlab����������git��ͬ�汾�������ݣ�������Ч��
���ܿ�̫��汾���Ի���������ĳ����汾�����һ���汾������
10.6.4ee--10.6.4ce--10.7.7ce--10.8.7ce--11.2.8ce--11.4.6ce
2.1�����ذ�
https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7/
2.2��ֹͣ��ط���
gitlab-ctl stop unicorn \
gitlab-ctl stop sidekiq \
gitlab-ctl stop nginx 
2.3��������װ
rpm -Uvh
#rpm -Uvh gitlab-ce-10.0.4-ce.0.el7.x86_64.rpm
2.4�� ��������gitlab
gitlab-ctl reconfigure
2.5�� ����gitlab
gitlab-ctl restart


��������gitlab
3.1�����������ļ�
gitlab-rake gitlab:backup:create
ʹ�������������/var/opt/gitlab/backupsĿ¼�´���ѹ�����ݰ�
3.2�����������ļ�
/etc/gitlab/gitlab.rb �����ļ��뱸��
/var/opt/gitlab/nginx/conf nginx�����ļ�
/etc/postfix/main.cfpostfix �ʼ����ñ���
�ġ�Ǩ��
4.1��ȷ����Gitlab����������Gitlab�������汾��ͬ
4.2���ϱ����ļ�Ŀ¼��/var/opt/gitlab/backupsĿ¼���µı����ļ��������·������ϵ�/var/opt/gitlab/backupsĿ¼
scp root@172.28.17.155:/var/opt/gitlab/backups/1502357536_2017_08_10_9.4.3_gitlab_backup.tar /var/opt/gitlab/backups/
4.3���ӱ����ļ��лָ�gitlab
4.3.1���������ļ�Ȩ���޸�Ϊ777
chmod 777 1502357536_2017_08_10_9.4.3_gitlab_backup.tar
4.3.2��ִ������ֹͣ����������ӷ���
gitlab-ctl stop unicorn
gitlab-ctl stop sidekiq
4.3.3��ִ������ӱ����ļ��лָ�Gitlab
#gitlab-rake gitlab:backup:restore BACKUP=�����ļ����
gitlab-rake gitlab:backup:restore BACKUP=1502357536_2017_08_10_9.4.3
�塢����git���󹦸��
sudo gitlab-ctl start

