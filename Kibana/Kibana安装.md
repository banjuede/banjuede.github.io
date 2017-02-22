#### 1. 解压安装
```
sha1sum kibana-5.1.1-linux-x86_64.tar.gz
tar -xzf kibana-5.1.1-linux-x86_64.tar.gz
cd kibana-5.1.1-linux-x86_64/
```

#### 2. 配置
以下几项需要配置
```
vi config/kibana.yml

server.host: 192.168.1.87
elasticsearch.url: http://192.168.1.87:9200/
```

#### 3. 启动
```
./bin/kibana
```