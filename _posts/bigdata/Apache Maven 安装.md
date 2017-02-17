先附[官方文档](https://maven.apache.org/install.html)

# 安装
maven的安装特别简单，只有两步

## 1. 下载解压
[下载地址](https://maven.apache.org/download.cgi)，例如下载 `apache-maven-3.3.9-bin.tar.gz`
```
解压
$ tar -zxf apache-maven-3.3.9-bin.tar.gz
```

## 2. 配置环境变量
```
$ vi /etc/profile
加入最后一行：
export PATH=$PATH:/root/apache-maven-3.3.9/bin
```

**测试一下**
```
$ mvn -v

Java HotSpot(TM) 64-Bit Server VM warning: ignoring option MaxPermSize=128m; support was removed in 8.0
Apache Maven 3.3.9 (bb52d8502b132ec0a5a3f4c09453c07478323dc5; 2015-11-11T00:41:47+08:00)
Maven home: /root/apache-maven-3.3.9
Java version: 1.8.0_60, vendor: Oracle Corporation
Java home: /usr/local/jdk/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "3.10.0-229.el7.x86_64", arch: "amd64", family: "unix"
```
