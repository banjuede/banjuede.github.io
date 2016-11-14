# Zepplin connect Spark on Yarn

Zeppelin 连接到现有的Spark on Yarn集群，Zeppelin 部署在单独的机器上，和Spark on Yarn没有关联。

解决方案：

> 1. install Zeppelin with proper params:
>```
> git clone https://github.com/apache/incubator-zeppelin.git ~/zeppelin
> cd ~/zeppelin
> mvn clean package -Pspark-1.4 -Dhadoop.version=2.6.0 -Phadoop-2.6 -Pyarn -DskipTests
>```
> 2. Update EMR_MASTER EC2 security groups to accept incoming requests from all ports, to communicate with Zeppelin (should be specific port, not yet know which)
> 3. Copy directory EMR_MASTER:/etc/hadoop/conf to MY_STANDALONE_SERVER:/home/zeppelin/hadoop-conf.
> 4. zeppelin/conf/zeppelin-env.sh should contain:
>```
> export MASTER=yarn-client
> export HADOOP_CONF_DIR=/home/zeppelin/hadoop-conf
>```
> Note: Spark parameters like spark.executor.instances are taken from Interpreter settings, is specified there.