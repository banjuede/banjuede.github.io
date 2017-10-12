# OpenTSDB 插入数据方式与格式

---

 - 使用 telnet 方式

```
put <metric> <timestamp> <value> <tagk_1>=<tagv_1>[ <tagk_n>=<tagv_n>]
```

 - 使用 HTTP API 方式

`http://host:port/api/put?details`
```
[
    {
        "metric": "sys.cpu.nice",
        "timestamp": 1346846400,
        "value": 18,
        "tags": {
           "host": "web01",
           "app": "lga"
        }
    },
    {
        "metric": "sys.cpu.nice",
        "timestamp": 1346846400,
        "value": 9,
        "tags": {
           "host": "web02",
           "app": "lga"
        }
    }
]
```
> Array大小 <= 50
 
