# Service REST API
---
## 获取Service的状态
>```
> curl -u admin:admin -H "X-Requested-By: ambari" -X GET  http://eds.zetyun.com:8080/api/v1/clusters/eds-cluster/services/JOBSERVER
>```

## 停止Service
>```
> curl -u admin:admin -H "X-Requested-By: ambari" -X PUT -d '{"RequestInfo":{"context":"Stop Service"},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}' http://eds.zetyun.com:8080/api/v1/clusters/eds-cluster/services/JOBSERVER
>```
 
## 删除Service
>```
> curl -u admin:admin -H "X-Requested-By: ambari" -X DELETE  http://eds.zetyun.com:8080/api/v1/clusters/eds-cluster/services/JOBSERVER
>```

---
# Extension REST API
---
## Get all extensions
>```
> curl -u admin:admin -H 'X-Requested-By:ambari' -X GET 'http://base1:8080/api/v1/extensions'
>```

## Get extension
>```
> curl -u admin:admin -H 'X-Requested-By:ambari' -X GET 'http://base1:8080/api/v1/extensions/EDS'
>```

## Get extension version
>```
> curl -u admin:admin -H 'X-Requested-By:ambari' -X GET 'http://base1:8080/api/v1/extensions/EDS/versions/1.0'
>```


---
# Extension Link REST API
---
## Create Extension Link
>```
> curl -u admin:admin -H 'X-Requested-By: ambari' -X POST -d '{"ExtensionLink": {"stack_name": "HDP", "stack_version":"2.5", "extension_name": "EDS", "extension_version": "1.0"}}' http://base1:8080/api/v1/links/
>```

## Get All Extension Links
>```
> curl -u admin:admin -H 'X-Requested-By:ambari' -X GET 'http://base1:8080/api/v1/links'
>```

## Get Extension Link
>```
> curl -u admin:admin -H 'X-Requested-By:ambari' -X GET 'http://base1:8080/api/v1/link/<link_id>'
>```

## Delete Extension Link
>```
> curl -u admin:admin -H 'X-Requested-By: ambari' -X DELETE http://base1:8080/api/v1/links/<link_id>
>```

## Update All Extension Links
>```
> curl -u admin:admin -H 'X-Requested-By: ambari' -X PUT http://base1:8080/api/v1/links/
>```
