先附[官方文档](https://cwiki.apache.org/confluence/display/HAWQ/Build+and+Install)

特别声明：本文档适应于 Red Hat/CentOS 7.x 下进行编译安装

## 1. 先解决依赖的问题
>```
> yum install wget
> wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
> rpm -ivh epel-release-latest-7.noarch.rpm
> yum makecache
> yum install -y man passwd sudo tar which git mlocate links make bzip2 net-tools \
    autoconf automake libtool m4 gcc gcc-c++ gdb bison flex gperf maven indent \
    libuuid-devel krb5-devel libgsasl-devel expat-devel libxml2-devel \
    perl-ExtUtils-Embed pam-devel python-devel libcurl-devel snappy-devel \
    thrift-devel libyaml-devel libevent-devel bzip2-devel openssl-devel \
    openldap-devel protobuf-devel readline-devel net-snmp-devel apr-devel \
    libesmtp-devel python-pip json-c-devel \
    java-1.7.0-openjdk-devel lcov cmake\
    openssh-clients openssh-server perl-JSON
> pip --retries=50 --timeout=300 install pycrypto
> pip install paramiko
>```

## 2. 设置系统参数
vi /etc/sysctl.conf
>```
> kernel.shmmax = 1000000000
> kernel.shmmni = 4096
> kernel.shmall = 4000000000
> kernel.sem = 250 512000 100 2048
> kernel.sysrq = 1
> kernel.core_uses_pid = 1
> kernel.msgmnb = 65536
> kernel.msgmax = 65536
> kernel.msgmni = 2048
> net.ipv4.tcp_syncookies = 0
> net.ipv4.ip_forward = 0
> net.ipv4.conf.default.accept_source_route = 0
> net.ipv4.tcp_tw_recycle = 1
> net.ipv4.tcp_max_syn_backlog = 200000
> net.ipv4.conf.all.arp_filter = 1
> net.ipv4.ip_local_port_range = 1281 65535
> vm.overcommit_memory = 2
> fs.nr_open = 3000000
> kernel.threads-max = 798720
> kernel.pid_max = 798720
> # increase network
> net.core.rmem_max=2097152
> net.core.wmem_max=2097
>```
应用系统参数：
>```
> sysctl -p
>```
vi /etc/security/limits.conf
>```
> * soft nofile 2900000
> * hard nofile 2900000
> * soft nproc 131072
> * hard nproc 131072
>```
到此系统参数设置成功，最好可以退出当前bash，重新登录

## 安装HADOOP
因为HAWQ是建立在HDFS上的，并且HAWQ不是通过HDFS的API进行访问的，而是通过libhdfs3.so进行访问的，所以在编译安装HAWQ之前一定要安装HDFS，YARN的安装是可选的。
HADOOP的安装自行解决，一定要保证HDFS可以正常工作，才可以继续往下编译

## 特别注意
**以上几个步骤一定要在安装HAWQ的所有节点上进行安装设置**

## 获取HAWQ代码并编译
>```
> git clone https://git-wip-us.apache.org/repos/asf/incubator-hawq.git
> cd ./incubator-hawq
> ./configure --prefix=hawq_install_path (hawq_install_path就是hawq要编译到的目标目录)
> make -j8
> make install
>```

## 编译HAWQ成功后进行配置
>```
> vi etc/hawq-site.xml
> 推荐配置项包括：
> hawq_master_address_host
> hawq_master_address_port
> hawq_dfs_url
> hawq_rm_yarn_address
> hawq_rm_yarn_scheduler_address
>```

## Init/Start/Stop HAWQ
>```
> source hawq_install_path/greenplum_path.sh
> hawq init cluster
> hawq start cluster
>```

## 配置用户
到此处本机是可以连接HAWQ的，但是远程不能连接，需要进行如下配置:
>```
> vi ${hawq_master_directory}/pg_hba.conf
> 用如下几行配置替换文件最后几行的配置
> local    all         gpadmin         ident
> host     all         gpadmin         127.0.0.1/28    trust
> host     all         all             0.0.0.0/0    trust
>```
这样远程就可以连接HAWQ了

## 测试HAWQ
vi testhawq.sql
>```
> create role postgres with SUPERUSER LOGIN CREATEEXTTABLE CREATEDB UNENCRYPTED PASSWORD 'postgres';
> CREATE DATABASE test101;
> CREATE TABLE test(id INT, name TEXT);
> INSERT INTO test values(1,'ansible test');
> SELECT COUNT(*) FROM test;
> EXPLAIN ANALYZE SELECT COUNT(*) FROM test;
> DROP TABLE test;
> DROP DATABASE test101;
>```
>```
> psql -h 127.0.0.1 -p 25432 -U gpadmin -d postgres -f testpg.sql
> -U 用户，-d 数据库
>```
