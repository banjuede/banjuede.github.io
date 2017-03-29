自从CarbonData 0.2.0之后解决了必须依赖于Apache Thrift的问题，这样就会方便很多，如果你需要修改Thrift的文件配置则还是需要安装Apache Thrift的，具体安装过程见[Apache Thrift 0.9.3 安装文档](http://www.jianshu.com/p/08c5d24656ae)

但在编译安装之前还是要做一些必要条件的：
```
Unix-like environment (Linux, Mac OS X)
Git
Apache Maven (Recommend version 3.3 or later) [安装文档](https://maven.apache.org/install.html)
Oracle Java 7 or 8
```
**Note: 编译安装CarbonData 0.2.0版本已经不需要安装Apache Thrift了**

# 1. 编译安装
[下载地址](https://www.apache.org/dyn/closer.lua/incubator/carbondata/0.2.0-incubating)
解压后执行以下命令（把解压后的目录重命名为carbondata）:
```
cd carbondata/
export MAVEN_OPTS="-Xmx512m -XX:MaxPermSize=128m" (可选)
mvn -DskipTests -Phadoop-2.7.2 -Pspark-1.6 -Dspark.version=1.6.2 -Pbuild-all install
```
(其中 -DskipTests 是跳过测试，-Phadoop-2.7.2 Hadoop版本，-Pspark-1.6 -Dspark.version=1.6.2 Spark版本)

# 2. 使用
```
cat > sample.csv << EOF
id,name,city,age
1,david,shenzhen,31
2,eason,shenzhen,27
3,jarry,wuhan,35
EOF

./bin/carbon-spark-shell
scala> cc.sql("create table if not exists test_table (id string, name string, city string, age Int) STORED BY 'carbondata'")
scala> import java.io.File
scala> val dataFilePath = new File("sample.csv").getCanonicalPath
scala> cc.sql(s"load data inpath '$dataFilePath' into table test_table")
```
