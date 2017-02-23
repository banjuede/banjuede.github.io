Flume中的拦截器（interceptor），用户Source读取events发送到Sink的时候，在events header中加入一些有用的信息，或者对events的内容进行过滤，完成初步的数据清洗。这在实际业务场景中非常有用，Flume-ng 1.6中目前提供了以下拦截器：
```
Timestamp Interceptor；
Host Interceptor；
Static Interceptor；
UUID Interceptor；
Morphline Interceptor；
Search and Replace Interceptor；
Regex Filtering Interceptor；
Regex Extractor Interceptor；
```
本文对常用的几种拦截器进行学习和介绍，并附上使用示例。

本文中使用的Source为TaildirSource，就是监控一个文件的变化，将内容发送给Sink，具体可参考《Flume中的TaildirSource》，Source配置如下：
```
agent_lxw1234.sources = src1
agent_lxw1234.channels = fileChannel
agent_lxw1234.sinks = sink1

# source 配置
agent_lxw1234.sources.src1.type = com.lumk.flume.TaildirSource
agent_lxw1234.sources.src1.positionFile = /tmp/flume/agent_lxw1234_position.json
agent_lxw1234.sources.src1.filegroups = f1
agent_lxw1234.sources.src1.filegroups.f1 = /tmp/lxw1234_.*.log
agent_lxw1234.sources.src1.batchSize = 100
agent_lxw1234.sources.src1.backoffSleepIncrement = 1000
agent_lxw1234.sources.src1.maxBackoffSleep = 5000
agent_lxw1234.sources.src1.channels = fileChannel
```
Flume Source中使用拦截器的相关配置如下：
```
## source 拦截器
agent_lxw1234.sources.sources1.interceptors = i1 i2
agent_lxw1234.sources.sources1.interceptors.i1.type = host
agent_lxw1234.sources.sources1.interceptors.i1.useIP = false
agent_lxw1234.sources.sources1.interceptors.i1.hostHeader = agentHost
agent_lxw1234.sources.sources1.interceptors.i2.type = timestamp
```
对一个Source可以使用多个拦截器。

###　Timestamp Interceptor

时间戳拦截器，将当前时间戳（毫秒）加入到events header中，key名字为：timestamp，值为当前时间戳。用的不是很多。比如在使用HDFS Sink时候，根据events的时间戳生成结果文件，hdfs.path = hdfs://cdh5/tmp/dap/%Y%m%d

hdfs.filePrefix = log_%Y%m%d_%H

会根据时间戳将数据写入相应的文件中。

但可以用其他方式代替（设置useLocalTimeStamp = true）。

### Host Interceptor

主机名拦截器。将运行Flume agent的主机名或者IP地址加入到events header中，key名字为：host（也可自定义）。

根据上面的Source，拦截器的配置如下：
```
## source 拦截器
agent_lxw1234.sources.sources1.interceptors = i1
agent_lxw1234.sources.sources1.interceptors.i1.type = host
agent_lxw1234.sources.sources1.interceptors.i1.useIP = false
agent_lxw1234.sources.sources1.interceptors.i1.hostHeader = agentHost

# sink 1 配置
agent_lxw1234.sinks.sink1.type = hdfs
agent_lxw1234.sinks.sink1.hdfs.path = hdfs://cdh5/tmp/lxw1234/%Y%m%d
agent_lxw1234.sinks.sink1.hdfs.filePrefix = lxw1234_%{agentHost}
agent_lxw1234.sinks.sink1.hdfs.fileSuffix = .log
agent_lxw1234.sinks.sink1.hdfs.fileType = DataStream
agent_lxw1234.sinks.sink1.hdfs.useLocalTimeStamp = true
agent_lxw1234.sinks.sink1.hdfs.writeFormat = Text
agent_lxw1234.sinks.sink1.hdfs.rollCount = 0
agent_lxw1234.sinks.sink1.hdfs.rollSize = 0
agent_lxw1234.sinks.sink1.hdfs.rollInterval = 600
agent_lxw1234.sinks.sink1.hdfs.batchSize = 500
agent_lxw1234.sinks.sink1.hdfs.threadsPoolSize = 10
agent_lxw1234.sinks.sink1.hdfs.idleTimeout = 0
agent_lxw1234.sinks.sink1.hdfs.minBlockReplicas = 1
agent_lxw1234.sinks.sink1.channel = fileChannel
```
该配置用于将source的events保存到HDFS上hdfs://cdh5/tmp/lxw1234的目录下，文件名为lxw1234_<主机名>.log

### Static Interceptor

静态拦截器，用于在events header中加入一组静态的key和value。

根据上面的Source，拦截器的配置如下：
```
## source 拦截器
agent_lxw1234.sources.sources1.interceptors = i1
agent_lxw1234.sources.sources1.interceptors.i1.type = static
agent_lxw1234.sources.sources1.interceptors.i1.preserveExisting = true
agent_lxw1234.sources.sources1.interceptors.i1.key = static_key
agent_lxw1234.sources.sources1.interceptors.i1.value = static_value

# sink 1 配置
agent_lxw1234.sinks.sink1.type = hdfs
agent_lxw1234.sinks.sink1.hdfs.path = hdfs://cdh5/tmp/lxw1234
agent_lxw1234.sinks.sink1.hdfs.filePrefix = lxw1234_%{static_key}
agent_lxw1234.sinks.sink1.hdfs.fileSuffix = .log
agent_lxw1234.sinks.sink1.hdfs.fileType = DataStream
agent_lxw1234.sinks.sink1.hdfs.useLocalTimeStamp = true
agent_lxw1234.sinks.sink1.hdfs.writeFormat = Text
agent_lxw1234.sinks.sink1.hdfs.rollCount = 0
agent_lxw1234.sinks.sink1.hdfs.rollSize = 0
agent_lxw1234.sinks.sink1.hdfs.rollInterval = 600
agent_lxw1234.sinks.sink1.hdfs.batchSize = 500
agent_lxw1234.sinks.sink1.hdfs.threadsPoolSize = 10
agent_lxw1234.sinks.sink1.hdfs.idleTimeout = 0
agent_lxw1234.sinks.sink1.hdfs.minBlockReplicas = 1
agent_lxw1234.sinks.sink1.channel = fileChannel
```
看看最终Sink在HDFS上生成的文件结构：
![图1](/image/Flume中的拦截器（Interceptor）介绍与使用-1.jpg)

### UUID Interceptor

UUID拦截器，用于在每个events header中生成一个UUID字符串，例如：b5755073-77a9-43c1-8fad-b7a586fc1b97。生成的UUID可以在sink中读取并使用。根据上面的source，拦截器的配置如下：
```
## source 拦截器
agent_lxw1234.sources.sources1.interceptors = i1
agent_lxw1234.sources.sources1.interceptors.i1.type = org.apache.flume.sink.solr.morphline.UUIDInterceptor$Builder
agent_lxw1234.sources.sources1.interceptors.i1.headerName = uuid
agent_lxw1234.sources.sources1.interceptors.i1.preserveExisting = true
agent_lxw1234.sources.sources1.interceptors.i1.prefix = UUID_

# sink 1 配置
agent_lxw1234.sinks.sink1.type = logger
agent_lxw1234.sinks.sink1.channel = fileChannel
```
运行后在日志中查看header信息：
![图2](/image/Flume中的拦截器（Interceptor）介绍与使用-2.jpg)

### Morphline Interceptor

Morphline拦截器，该拦截器使用Morphline对每个events数据做相应的转换。关于Morphline的使用，可参考

http://kitesdk.org/docs/current/morphlines/morphlines-reference-guide.html

### Search and Replace Interceptor

该拦截器用于将events中的正则匹配到的内容做相应的替换。

具体配置示例如下：
```
## source 拦截器
agent_lxw1234.sources.sources1.interceptors = i1
agent_lxw1234.sources.sources1.interceptors.i1.type = search_replace
agent_lxw1234.sources.sources1.interceptors.i1.searchPattern = [0-9]+
agent_lxw1234.sources.sources1.interceptors.i1.replaceString = lxw1234
agent_lxw1234.sources.sources1.interceptors.i1.charset = UTF-8

# sink 1 配置
##agent_lxw1234.sinks.sink1.type = com.lxw1234.sink.MySink
agent_lxw1234.sinks.sink1.type = logger
agent_lxw1234.sinks.sink1.channel = fileChannel
```
该配置将events中的数字替换为lxw1234。

原始的events内容为：
![图3](/image/Flume中的拦截器（Interceptor）介绍与使用-3.jpg)

实际的events内容为：
![图4](/image/Flume中的拦截器（Interceptor）介绍与使用-4.jpg)

### Regex Filtering Interceptor

该拦截器使用正则表达式过滤原始events中的内容。

配置示例如下：
```
## source 拦截器
agent_lxw1234.sources.sources1.interceptors = i1
agent_lxw1234.sources.sources1.interceptors.i1.type = regex_filter
agent_lxw1234.sources.sources1.interceptors.i1.regex = ^lxw1234.*
agent_lxw1234.sources.sources1.interceptors.i1.excludeEvents = false

# sink 1 配置
##agent_lxw1234.sinks.sink1.type = com.lxw1234.sink.MySink
agent_lxw1234.sinks.sink1.type = logger
agent_lxw1234.sinks.sink1.channel = fileChannel
```
该配置表示过滤掉不是以lxw1234开头的events。

如果excludeEvents设为true，则表示过滤掉以lxw1234开头的events。

原始events内容为：
![图5](/image/Flume中的拦截器（Interceptor）介绍与使用-5.jpg)

拦截后的events内容为：
![图6](/image/Flume中的拦截器（Interceptor）介绍与使用-6.jpg)

### Regex Extractor Interceptor

该拦截器使用正则表达式抽取原始events中的内容，并将该内容加入events header中。

配置示例如下：
```
## source 拦截器
agent_lxw1234.sources.sources1.interceptors = i1
agent_lxw1234.sources.sources1.interceptors.i1.type = regex_extractor
agent_lxw1234.sources.sources1.interceptors.i1.regex = cookieid is (.*?) and ip is (.*?)
agent_lxw1234.sources.sources1.interceptors.i1.serializers = s1 s2
agent_lxw1234.sources.sources1.interceptors.i1.serializers.s1.type = default
agent_lxw1234.sources.sources1.interceptors.i1.serializers.s1.name = cookieid
agent_lxw1234.sources.sources1.interceptors.i1.serializers.s2.type = default
agent_lxw1234.sources.sources1.interceptors.i1.serializers.s2.name = ip

# sink 1 配置
##agent_lxw1234.sinks.sink1.type = com.lxw1234.sink.MySink
agent_lxw1234.sinks.sink1.type = logger
agent_lxw1234.sinks.sink1.channel = fileChannel
```
该配置从原始events中抽取出cookieid和ip，加入到events header中。

原始的events内容为：
![图7](/image/Flume中的拦截器（Interceptor）介绍与使用-7.jpg)

events header中的内容为：
![图8](/image/Flume中的拦截器（Interceptor）介绍与使用-8.jpg)


Flume的拦截器可以配合Sink完成许多业务场景需要的功能，比如：按照时间及主机生成目标文件目录及文件名；配合Kafka Sink完成多分区的写入等等。
