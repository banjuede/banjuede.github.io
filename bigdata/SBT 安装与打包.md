## 1. SBT 地位
SBT就像maven一样是一个scala项目的构建工具，我也只是会简单的使用打包，下面我会介绍一下SBT整个的安装流程和打包过程，其实很简单。

## 2. SBT 安装
像maven一样SBT是一个软件所以需要在系统上安装此工具，[官网下载](http://www.scala-sbt.org/)

## 3. 修改SBT jar包下载目录
**windows下**

修改sbt安装目录下conf目录下的sbtconfig.txt
```
C:\Program Files (x86)\sbt\conf\sbtconfig.txt

# 添加以下两行配置，可以把原来的目录复制一份
-Dsbt.boot.directory=D:/.sbt/boot
-Dsbt.ivy.home=D:/.sbt/.ivy2
```