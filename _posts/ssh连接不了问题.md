ssh连接不了问题
1、查看是否正常是否报错，文件相关权限是否正常,不提示就是正常。
[root@izysl2jv0d6fukz ssh]# sshd -t
2、更改相关文件权限，不要都设置为755或者777权限。
[root@izysl2jv0d6fukz ssh]# cd /etc/ssh
[root@izysl2jv0d6fukz ssh]# ll
[root@izysl2jv0d6fukz ssh]# chmod 644 moduli ssh_config ssh_host_dsa_key.pub ssh_host_ecdsa.pub ssh_host_ed25519_key.pub ssh_host_rsa_key.pub
[root@izysl2jv0d6fukz ssh]# chmod 600 ssh_host_dsa_key ssh_host_ed25519_key ssh_host_ecdsa_key ssh_host_rsa_key
[root@izysl2jv0d6fukz ssh]# sshd -t
[root@izysl2jv0d6fukz ssh]# systemctl restart sshd
3、查看端口
[root@izysl2jv0d6fukz ssh]# netstat -ntalp | grep ssh
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      6567/sshd
tcp        0     52 172.18.28.30:22         61.242.42.242:42877     ESTABLISHED 9400/sshd: root@pts
3、查看权限
[root@izysl2jv0d6fukz ssh]# cd /etc/ssh
[root@izysl2jv0d6fukz ssh]# ll
total 604
total 612
-rw-r--r-- 1 root root     581843 Apr 11  2018 moduli
-rw-r--r-- 1 root root       2276 Apr 11  2018 ssh_config
-rw------- 1 root root       3668 Dec  3 20:08 sshd_config
-rw------- 1 root root        668 Dec  3 20:09 ssh_host_dsa_key
-rw-r--r-- 1 root root        610 Dec  3 20:09 ssh_host_dsa_key.pub
-rw------- 1 root root        227 Dec  3 20:09 ssh_host_ecdsa_key
-rw-r--r-- 1 root root        182 Dec  3 20:09 ssh_host_ecdsa_key.pub
-rw-r----- 1 root ssh_keys    387 Dec  3 20:09 ssh_host_ed25519_key
-rw-r--r-- 1 root root         82 Dec  3 20:09 ssh_host_ed25519_key.pub
-rw------- 1 root root       1675 Dec  3 20:09 ssh_host_rsa_key
-rw-r--r-- 1 root root        402 Dec  3 20:09 ssh_host_rsa_key.pub
