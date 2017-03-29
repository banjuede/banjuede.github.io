## 安装前

* A Linux system \(or Windows with manual building\)
* Java Runtime Environment 1.6 or later
* HBase 0.92 or later
* GnuPlot 4.2 or later

* **下载安装包**

```
https://github.com/OpenTSDB/opentsdb/releases
# 下载 opentsdb-2.3.0.rpm 包，复制到 /root 目录下
```

* **安装GnuPlot**

```
$ yum install gnuplot
```

## 安装配置

* **安装**

```
$ rpm -ivh opentsdb-2.3.0.rpm
```

opentsdb会安装到 /usr/share/ 下

* **配置**

```
$ cd /usr/share/opentsdb
$ vim etc/opentsdb/opentsdb.conf
# 需要修改的配置项
tsd.storage.hbase.zk_basedir = /hbase-unsecure (这个配置是Hbase在zookeeper中的znode路径)
tsd.storage.hbase.zk_quorum = 192.168.1.68 (这个配置是Hbase所在的zookeeper server的地址)
tsd.core.auto_create_metrics = true (是否自动创建metric)
```

查询Hbase在Zookeeper中znode路径：

```
$ cd /usr/hdp/2.3.4.0-3485/zookeeper/bin
$ ./zkCli.sh

[zk: localhost:2181(CONNECTED) 1] ls /
[hiveserver2, zookeeper, hbase-unsecure, rmstore]
[zk: localhost:2181(CONNECTED) 2]
```

可见Hbase在zookeeper中znode的路径为 /hbase-unsecure

* **在Hbase中创建表**

```
$ cd /usr/share/opentsdb
$ env COMPRESSION=NONE HBASE_HOME=/usr/hdp/2.3.4.0-3485/hbase ./tools/create_table.sh
```

COMPRESSION的值有四个选项：`NONE, LZO, GZIP, SNAPPY`

这条命令将会在Hbase中创建四个表：`tsdb, tsdb-uid, tsdb-tree, tsdb-meta`

## 启动

```
$ cd /usr/share/opentsdb
$ bash bin/tsdb tsd &
[1] 251022
```

启动完成以后访问 http://192.168.1.68:4242/ \(默认端口为4242\)
