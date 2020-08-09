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
分词
```
late night，会被拆解成late和night
latenight，会被认为一个单词，当搜索late是无法命中
```
索引方式
- analyzed：默认方式，会进行分词
- not_analyzed：不会进行分词，只有精确匹配可以命中，比如big data，只能被big data命中，不能被data命中
- no：不进行索引，无法根据该字段搜索，但可以出现在搜索结果中