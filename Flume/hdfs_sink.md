### 示例一
```
agent1.channels=ch1
agent1.channels.ch1.capacity=100000
agent1.channels.ch1.keep-alive=30
agent1.channels.ch1.transactionCapacity=100000
agent1.channels.ch1.type=memory

agent1.sinks=sink1
agent1.sinks.sink1.channel=ch1
agent1.sinks.sink1.type=hdfs
agent1.sinks.sink1.hdfs.filePrefix=log
agent1.sinks.sink1.hdfs.path=hdfs://base1.zetyun.com:8020/log_monitor/%y-%m-%d/
agent1.sinks.sink1.hdfs.round=true
agent1.sinks.sink1.hdfs.roundUnit=minute
agent1.sinks.sink1.hdfs.roundValue=10

agent1.sources=source1
agent1.sources.source1.channels=ch1
agent1.sources.source1.command=tail -n +0 -F /mnt/eds/eds_components/a3/logs/a3.log
agent1.sources.source1.shell=/bin/bash -c
agent1.sources.source1.threads=5
agent1.sources.source1.type=exec
```

### 示例二
```
agent1.channels=ch1
agent1.channels.ch1.checkpointDir=/mnt/flume/checkpoint
agent1.channels.ch1.dataDirs=/mnt/flume/data
agent1.channels.ch1.type=file

agent1.sinks=sink_hdfs
agent1.sinks.sink_hdfs.channel=ch1
agent1.sinks.sink_hdfs.hdfs.batchSize=1000
agent1.sinks.sink_hdfs.hdfs.filePrefix=logs
agent1.sinks.sink_hdfs.hdfs.fileType=DataStream
agent1.sinks.sink_hdfs.hdfs.inUsePrefix=.
agent1.sinks.sink_hdfs.hdfs.path=hdfs://base1.zetyun.com:8020/log_monitor/%Y-%m-%d
agent1.sinks.sink_hdfs.hdfs.rollCount=0
agent1.sinks.sink_hdfs.hdfs.rollInterval=60
agent1.sinks.sink_hdfs.hdfs.rollSize=0
agent1.sinks.sink_hdfs.hdfs.writeFormat=text
agent1.sinks.sink_hdfs.type=hdfs

agent1.sources=src1
agent1.sources.src1.channels=ch1
agent1.sources.src1.deletePolicy=never
agent1.sources.src1.fileHeader=true
agent1.sources.src1.interceptors=i1
agent1.sources.src1.interceptors.i1.type=timestamp
agent1.sources.src1.spoolDir=/mnt/log_monitor
agent1.sources.src1.type=spooldir
```
