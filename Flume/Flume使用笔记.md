## hdfs-sink

#### 1. hdfs用户下创建Hdfs目录
```
su - flume -c "hdfs dfs -mkdir /log_monitor/"
su - flume -c "mkdir /mnt/log_monitor"
su - flume -c "mkdir /mnt/flume"
```

#### 2. 采集目录到HDFS
```
agent1.sources.src1.channels = ch1
agent1.sources.src1.type = spooldir
agent1.sources.src1.spoolDir = /mnt/log_monitor
agent1.sources.src1.deletePolicy= never
agent1.sources.src1.fileHeader = true
agent1.sources.src1.interceptors =i1
agent1.sources.src1.interceptors.i1.type = timestamp

agent1.channels.ch1.type = file
agent1.channels.ch1.checkpointDir= /mnt/flume/checkpoint
agent1.channels.ch1.dataDirs= /mnt/flume/data

agent1.sinks.sink_hdfs.channel = ch1
agent1.sinks.sink_hdfs.type = hdfs
agent1.sinks.sink_hdfs.hdfs.path = hdfs://base1.zetyun.com:8020/log_monitor/%Y-%m-%d
agent1.sinks.sink_hdfs.hdfs.filePrefix = logs
agent1.sinks.sink_hdfs.hdfs.inUsePrefix = .
agent1.sinks.sink_hdfs.hdfs.rollInterval = 60
agent1.sinks.sink_hdfs.hdfs.rollSize = 0
agent1.sinks.sink_hdfs.hdfs.rollCount = 0
agent1.sinks.sink_hdfs.hdfs.batchSize = 1000
agent1.sinks.sink_hdfs.hdfs.writeFormat = text
agent1.sinks.sink_hdfs.hdfs.fileType = DataStream
#agent1.sinks.sink_hdfs.hdfs.fileType = CompressedStream
#agent1.sinks.sink_hdfs.hdfs.codeC = lzop

agent1.channels = ch1
agent1.sources = src1
agent1.sinks = sink_hdfs
```

#### 1.3 采集文件到HDFS
```
agent1.sources = source1
agent1.sinks = sink1
agent1.channels = channel1

# Describe/configure tail -F source1
agent1.sources.source1.type = exec
agent1.sources.source1.command = tail -n +0 -F /mnt/eds/eds_components/a3/logs/a3.log
agent1.sources.source1.channels = channel1

# Describe sink1
agent1.sinks.sink1.type = hdfs
#a1.sinks.k1.channel = c1
agent1.sinks.sink1.hdfs.path =hdfs://base1.zetyun.com:8020/log_monitor/a3/%y-%m-%d/
agent1.sinks.sink1.hdfs.filePrefix = a3_log
agent1.sinks.sink1.hdfs.maxOpenFiles = 5000
agent1.sinks.sink1.hdfs.batchSize= 100
agent1.sinks.sink1.hdfs.fileType = DataStream
agent1.sinks.sink1.hdfs.writeFormat =Text
agent1.sinks.sink1.hdfs.rollSize = 102400
agent1.sinks.sink1.hdfs.rollCount = 1000000
agent1.sinks.sink1.hdfs.rollInterval = 60
agent1.sinks.sink1.hdfs.round = true
agent1.sinks.sink1.hdfs.roundValue = 10
agent1.sinks.sink1.hdfs.roundUnit = minute
agent1.sinks.sink1.hdfs.useLocalTimeStamp = true

# Use a channel which buffers events in memory
agent1.channels.channel1.type = memory
agent1.channels.channel1.keep-alive = 120
agent1.channels.channel1.capacity = 500000
agent1.channels.channel1.transactionCapacity = 600

# Bind the source and sink to the channel
agent1.sources.source1.channels = channel1
agent1.sinks.sink1.channel = channel1
```

flume-ng agent -c /etc/flume-ng/conf -f /etc/flume-ng/conf/f2.conf -Dflume.root.logger=DEBUG,console -n agent-1

