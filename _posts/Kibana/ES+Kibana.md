# ES + Kibana
1. 关键字搜索用例
```
POST http://192.168.1.87:9200/pccc/_search
{
    "query": {
        "bool": {
            "must": [
                {
                    "term": {
                        "msgid.keyword": "5294c5bb-f75b-4e28-b4bc-23598e3d4e73"
                    }
                }
            ],
            "must_not": [],
            "should": []
        }
    },
    "from": 0,
    "size": 1,
    "sort": [],
    "aggs": {}
}
```

2. 通配符搜索用例
```
POST http://192.168.1.87:9200/pccc/_search
{
    "query": {
        "bool": {
            "must": [
                {
                    "wildcard": {
                        "channel.keyword": "touda.syslog.logical.com.pccc.pms.user.servicecustom.__process.USR.USR_008_0002.biz@*"
                    }
                }
            ],
            "must_not": [],
            "should": []
        }
    },
    "from": 0,
    "size": 1,
    "sort": [],
    "aggs": {}
}
```

3. 正则搜索用例
```
POST http://192.168.1.87:9200/pccc/_search
{
    "query": {
        "bool": {
            "must": [
                {
                    "regexp": {
                        "channel.keyword": "touda.syslog.logical.com.pccc.pms.user.servicecustom.__process.USR.USR_008_[0-9].+.biz@角色信息"
                    }
                }
            ],
            "must_not": [],
            "should": []
        }
    },
    "from": 0,
    "size": 10,
    "sort": [],
    "aggs": {}
}
```

#### Kibana 画图
```
某一个IP访问某一个APPNAME的servicename的前五名
remoteIP:182.180.117.124 AND appName:pms_user_service_sit
```

#### Discover
```
appName:pms_user_service_sit AND duration:(>1000)
```