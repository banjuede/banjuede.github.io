## 示例一
```
agent1.sources = src1
agent1.channels = ch1
agent1.sinks = sink1

##source配置
agent1.sources=src1
agent1.sources.src1.channels=ch1
agent1.sources.src1.deletePolicy=never
agent1.sources.src1.fileHeader=true
agent1.sources.src1.interceptors=i1
agent1.sources.src1.interceptors.i1.type=timestamp
agent1.sources.src1.spoolDir=/mnt/log_monitor
agent1.sources.src1.type=spooldir
agent1.sources.src1.ignorePattern = ^(.)*\\.tmp$
agent1.sources.src1.deserializer.maxLineLength = 4096

# channel配置
agent1.channels=ch1
agent1.channels.ch1.checkpointDir=/mnt/flume/checkpoint
agent1.channels.ch1.dataDirs=/mnt/flume/data
agent1.channels.ch1.type=file

# sink配置
agent1.sinks.sink1.channel = ch1
agent1.sinks.sink1.type = org.apache.flume.sink.kafka.KafkaSink
# flume 1.7
#agent1.sinks.sink1.kafka.topic = log_monitor
#agent1.sinks.sink1.kafka.bootstrap.servers = base1:9092,base2:9092,base3:9092
#agent1.sinks.sink1.kafka.flumeBatchSize = 20
#agent1.sinks.sink1.kafka.producer.acks = 1
#agent1.sinks.sink1.kafka.producer.linger.ms = 1
#agent1.sinks.sink1.kafka.producer.compression.type = snappy

agent1.sinks.sink1.brokerList = base1:9092,base2:9092,base3:9092
agent1.sinks.sink1.topic = log_monitor
agent1.sinks.sink1.batchSize = 100
agent1.sinks.sink1.requiredAcks = 1
```
Note:`brokerList`需要参考`server.properties`中`listeners=PLAINTEXT://base1.zetyun.com:6667`，其端口有可能不是9092