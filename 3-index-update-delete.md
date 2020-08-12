# 映射
## 自动创建映射
当索引一篇文档的时候，ES会自动判断并创建映射关系  
```
// 索引一个doc
PUT http://{{host}}/get-together-1/_doc/1
{
    "name": "Late Night with ElasticSearch",
    "date": "2013-10-25T19:00:00"
}

// 查询它的mapping
GET http://{{host}}/get-together-1/_mapping
```
## 添加新的映射
```
PUT http://{{host}}/get-together-1/_mapping
{
    "properties": {
        "host": {
            "type": "text",
            "fields": {
                "keyword": {
                    "type": "keyword",
                    "ignore_above": 256
                }
            }
        }
    }
}
```
## 修改映射
通常，我们无法修改现有字段的类型以及被索引的方式  
可行的方案是，删除该索引下所有的数据，修改映射，重新索引数据
# 数据类型
四大核心类型
- 字符串
- 数值
- 日期
- 布尔

## 字符串
字符串有两种type，text和keyword
- text可以进行模糊匹配；分词
- keyword用于精准查询；不分词

see more [strings-are-dead](https://www.elastic.co/blog/strings-are-dead-long-live-strings)

```
// 分词
late night，会被拆解成late和night
latenight，会被认为一个单词，当搜索late是无法命中
```

## 数字
同样分浮点数和整数类型，默认整数类型分配long型索引，浮点数分配double型索引

## 日期
ES解析date类型并存储为long型（1970-1-1至今）  
日期格式默认使用 [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)
可以在mapping内自定义日期格式

## 数组
ES没有特殊的数组类型，所有数据类型默认支持单个或者数组。
```
// 可以索引一个数组类型的date
{
    "name": "Late Night with ElasticSearch",
    "date": ["2015-10-25T19:00:00", "2015-10-26T19:00:00"],
    "tags": ["first", "initial"]
}
// 也可以索引一个单值的date
{
    "name": "Late Night with ElasticSearch",
    "date": "2015-10-26T19:00:00",
    "tags": ["first", "initial"]
}
// 对应date的mapping只是普通的date
"date": {
    "type": "date"
}
```

# 更新
[Update API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-update.html)
```
// create a document
{
    "name": "Late Night with ElasticSearch",
    "date": "2015-10-26T19:00:00"
}

// update
// use script to add a field to _source
POST http://{{host}}/get-together-1/_update/Sbi15HMBtUE9uNYxz816/
{
    "script": {
        "source": "ctx._source.tags = \"first\""
    }
}

// use doc to update the fields
POST http://{{host}}/get-together-1/_update/Sbi15HMBtUE9uNYxz816/
{
    "doc": {
        "tags": "2nd"
    }
}
```
__版本控制__
使用_version字段实现乐观锁机制的版本控制

# 删除
删除文档  
`DELETE http://{{host}}/get-together-1/_doc/Sbi15HMBtUE9uNYxz816`
同样，整个索引也可以删除  
`DELETE http://{{host}}/get-together-2`  
再次进行搜索，会得到报错
```
{
    "type": "index_not_found_exception",
    "reason": "no such index [get-together-2]",
    "resource.type": "index_or_alias",
    "resource.id": "get-together-2",
    "index_uuid": "_na_",
    "index": "get-together-2"
}
```
__关闭索引__
对于暂时不想用的索引，可以使用关闭操作，关闭后无法写入文档和检索
```
// close
POST http://{{host}}/get-together-2/_close

{
    "acknowledged": true,
    "shards_acknowledged": true,
    "indices": {
        "get-together-2": {
            "closed": true
        }
    }
}

// re-open
POST http://{{host}}/get-together-2/_open
```