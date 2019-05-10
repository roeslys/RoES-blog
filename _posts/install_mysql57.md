```shell
Install_MySQL_57()
{
    Echo_Blue "[+] Installing ${Mysql_Ver}..."
    rm -f /etc/my.cnf
    Tar_Cd ${Mysql_Ver}.tar.gz ${Mysql_Ver}
    Install_Boost
    cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql -DSYSCONFDIR=/etc -DWITH_MYISAM_STORAGE_ENGINE=1 -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_PARTITION_STORAGE_ENGINE=1 -DWITH_FEDERATED_STORAGE_ENGINE=1 -DEXTRA_CHARSETS=all -DDEFAULT_CHARSET=utf8mb4 -DDEFAULT_COLLATION=utf8mb4_general_ci -DWITH_EMBEDDED_SERVER=1 -DENABLED_LOCAL_INFILE=1 ${MySQL_WITH_BOOST}
    Make_Install

groupadd mysql
useradd -s /sbin/nologin -M -g mysql mysql

cat > /etc/my.cnf<<EOF
```



```shell
Install_MySQL_51()
{
    Echo_Blue "[+] Installing ${Mysql_Ver}..."
    rm -f /etc/my.cnf
    Tar_Cd ${Mysql_Ver}.tar.gz ${Mysql_Ver}
    MySQL_Gcc7_Patch
    if [ "${InstallInnodb}" = "y" ]; then
        ./configure --prefix=/usr/local/mysql --with-extra-charsets=complex --enable-thread-safe-client --enable-assembler --with-mysqld-ldflags=-all-static --with-charset=utf8 --enable-thread-safe-client --with-big-tables --with-readline --with-ssl --with-embedded-server --enable-local-infile --with-plugins=innobase ${MySQL51MAOpt}
    else
        ./configure --prefix=/usr/local/mysql --with-extra-charsets=complex --enable-thread-safe-client --enable-assembler --with-mysqld-ldflags=-all-static --with-charset=utf8 --enable-thread-safe-client --with-big-tables --with-readline --with-ssl --with-embedded-server --enable-local-infile ${MySQL51MAOpt}
    fi
    sed -i '/set -ex;/,/done/d' Makefile
    Make_Install
    cd ../

groupadd mysql
useradd -s /sbin/nologin -M -g mysql mysql

cat > /etc/my.cnf<<EOF
```

