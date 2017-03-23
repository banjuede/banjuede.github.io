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

### 修改左上角Kibana图标
```
kibana/optimize/bundles/src/ui/public/images/kibana.svg
```

### 修改Loading Kibana图标
```
vi kibana/src/ui/views/ui_app.jade

找到117行，修改 Loading Kibana 为 Loading DataCanvas
```
87(在spark用户下)上有改好的`Kibana UI`，在其他机器上安装完后可以直接替换`src`目录