### Mapping date datatype

参考链接：https://www.elastic.co/guide/en/elasticsearch/reference/current/date.html

**mapping：**@timestamp type date

 range 时值的格式是yyyy-MM-dd HH:mm:ss，但是由于mapping中@timestamp字段类型是date，但是并没有进行设置格式化，如下：

```
"@timestamp": {
          "type":   "date",
          "format": "yyyy-MM-dd HH:mm:ss||yyyy-MM-dd||epoch_millis"
}
```

**结论：**

如果@timestamp字段在mapping中没有设置格式化，在检索的时候是需要加上format的，否则只能细化到yyyy-MM-dd的检索。如果@timestmp字段在mapping中设置了格式化，在检索的时候就不需要进行格式化了，直接传格式正确的值就好了！



