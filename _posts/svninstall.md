
##һ������VisualSVN��Ŀ
1. ����Ҫʹ��Linux��Ϊsvn��������֮ǰ����windows Server 2008�ϵģ��õ���VisualSVN�����߳���Ǩ����svn����������gitlab�ȣ�Ϊ�˿��ǲ����˺ܶ����ϣ�Ϊ����һ���ܽᣬϣ���Ժ�������������������Ͽɲ飬����Ȥ�Ŀ��Կ����ҵ�������ʡ�
2. ����svn����һ����Ŀfpp����Ҫ��fpp�����Ŀ������linux�����¡�����cmd���������� svnadmin dump E:\Repositories\fpp > e:\svnbak\fpp.dump����Ŀ������e:\svnbakĿ¼�¡�
3. �ɼ�����ᵼ��ÿ���汾����ϸ��Ϣ����֤��֮ǰ����ʷ��Ϣ���ᶪʧ���������Ǳ�õ������ļ�fpp.dump.
##�����ϴ������ļ���Linux
1. ����ftp, ssh��scp�ȹ��߽�fpp.dump�ļ����䵽Linux�������ϣ���������ֱ������openSSH�������ϴ����ļ���·��Ϊ/root/fpp.dump��
##����Linux��SVN�İ�װ������
1. Linux�°�װsvn������ֱ������yum������а�װ��`yum install subversion`���subversion�İ�װ��
2. �����汾��Ŀ¼svndata��
`mkdir /svndata`
`svnserve -d -r /svndata` #����svn�����ð汾��Ŀ¼Ϊ/svndata
> ��ʱ����ʧ�ܵĻ����鿴�˿��Ƿ�ռ�ã�`kill -9 1524`(���̺�)��ɱ����������������
` svnserve -d -r /svndata `
��֪�����Ǹ�����Ҳ����ֱ��ɱ�����н���
`killall svnserve` #�ر�svn
3. ������Ŀ��
`svnadmin create /svndata/fpp`  #fpp���������Ŀ��������Ժ�Ҫ�õ�
4. �����û�����Ȩ��
`cd /svndata/fpp/conf`
`vi svnserve.conf`
> �ͷ����¼��е�ע�� ,֮ǰ����������һЩʲô�������͸����Ϸ������е���˵���⼸��ע���ˣ��е��ǵ�һ�����Ϊread�Ļ��Ͳ��������˺��ˡ�
anon-access = none    #�����û����ɶ�д���˶���Ҫ����Ȩ�ޱ�������Ϊnone
auth-access = write    #��Ȩ�û���д
password-db = passwd  #���ĸ��ļ���Ϊ�û������ļ�
authz-db = authz    #���ĸ��ļ���ΪȨ���ļ�
��������Ҳ����д����·��������д����·����
password-db = /svndata/fpp/conf/passwd
authz-db =  /svndata/fpp/conf/authz
5. ���ӷ����û�����ʽΪ��username = password��
> �ر�ע�⣡�����Ⱥ�����Ҫ�ӿո񣬷�����Ч��û�мӿո񣬾�һֱû�ã���linux���������ļ��ﶼ��ע��
##�ġ����뱸���ļ�
1. ������� ` svnadmin load /svndata/fpp < /root/fpp.dump `
##�塢�ͻ��˽��д���ļ��
1.windows�˰�װTortoiseSVN, �Ҽ�svn checkout
2. �ڴ򿪵ĶԻ����У�����svn��ĵ�ַ��ȷ�������ͬ����Ŀ��ip��ַ����Ŀ���ơ�
3. svn��ʾ����ɹ�����Ŀ¼�¿����ҵ��������Ŀ��
4. ������ǰ����Ŀ���ض����µ�svn���������Ҽ�->TortoiseSVN->Relocate���ڵ����ĶԻ�������д�µĵ�ַ��TortoiseSVN����ʾ�޸ĳɹ���֮�󣬾Ϳ���ʹ���µ�svn�ˡ�

���������ܽ����£�
1����֪������ô���� svn://url ���·��
2��������Ҫ���õ��ļ�������authz��������[repos:/]������׸���ô����
3����֤ʧ������������
4��svn import Ŀ¼1 "svn://localhost/Ŀ¼2" -m "first version" Ŀ¼2������ô���ã�
5��import ��ʱ����֡���Ŀ�ӱ��ر���ת����UTF8ʧ�ܡ�
6���������˶�û�����ˣ����ǿͻ��˲�����������
����͸����⼸�����⣬һһ���

1��svn���Է�Ϊ���������汾�⣬���裺
�汾��Ŀ¼Ϊ /data/svndata/repos1
������������ǣ�svnserve -d -r /data/svndata/repos1  
������㵱ǰsvnֻΪrepos1����汾�⹤�����ͻ��˷���ֱ��svn://IP/ �Ϳ����ˣ����治��Ŀ¼
������������ǣ�svnserve -d -r /data/svndata/          ������㵱ǰsvn���Զ�汾�����У��ͻ��˷��ʾ���Ҫ���� svn://IP/repos1 �������ܷ���repos1�汾��
2����һ�������Ƕ�Ӧ��
�����һ���汾�⣬��Ӧ�����ó����£�
[groups]
admin = user1,user2
[/]
@admin=rw
����Ƕ���汾�⣬�Ǿ�Ӧ�����ó�������
[groups]
admin = user1,user2
[repos1:/]
@admin = rw
3����֤ʧ�ܵ����⣬���Ƕ�������������û�����Ӧ�����úã�Ҫô����һ���汾�����ã�Ҫô��������汾�����ã�ֻҪ��Ӧ���úã�Ӧ�þ���û������ġ�
4��Ŀ¼2����svn�����ģ������Լ�ȥ���ã����裺
svn import /tmp/ceshi "svn://localhost/a/b/c" -m "first version"
�����Ļ�������checkout��ʱ���㱾�ص�Ŀ¼��Ӧ����: /a/b/c
5�����϶�˵��LANGû���úã������ҵĲ���������⣬�ҵ��ǵ����Դ�ļ�����Щ�ļ�������ļ������룬����ʹ��sublime����Ҫ��notepad��
6�������������ú��ˣ���Ҫ�ǿͻ��˻������ϣ����Ƿ���ǽ�������ˣ�ȥ/etc/sysconfig/iptables ����һ�£���Ĭ�ϵ�3690�˿ھͿ�����

�ο�ԭ�ĵ�ַ��https://www.cnblogs.com/lidabo/p/4633152.html
https://www.cnblogs.com/lxwphp/p/8031811.html



