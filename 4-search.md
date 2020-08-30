# 搜索
## URL based search
基于URL的搜索提供了基本的功能，方便配合CURL命令
```
POST or GET
/_search // find all documents
/{{index}}/_search // find the documents of indicated index
/{{index}}/_search?size=10 // only return at most 10 documents of index
/{{index}}/_search?sort={{field}}:asc // sort by field in ascend
/{{index}}/_search?sort={{field}}:desc // sort by field in descend
/{{index}}/_search?_source={{field1}},{{field2}} // only return field1 & field2 of found documents
/{{index}}/_search?q={{field}}:{{value}} // filter, only find the document within field = value
```
## Request body based search
基于请求体的搜索提供了高级功能
```
GET kibana_sample_data_ecommerce/_search
{
  "query": {
    "match_all": {}
  },
  "size": 2,
  "_source": ["order_id", "order_date", "currency"],
  "sort": [
    {
      "order_date": {
        "order": "desc"
      }
    }
  ]
}
```
## 基础查询
- 使用query DSL
- 查询与过滤的区别，查询会对文档进行打分，根据打分结果返回最匹配的；而过滤只需要确定文档是否符合查询条件，所以更快  

__match__
```
// match, same as include
// to find document within currency = EUR
"match": {
    "currency":"EUR"
}
```
__match_all__  
it matches all documents  
__query_string__  
```
// 非常强大的查询方式，可以提供AND,OR的条件连接
// 由于过去强大，使得查询表达式在条件过多时难以阅读
"query": {
    "query_string": {
      "default_field": "category",
      "query": "Men*"
    }
}
```
__term & terms__
```
// term, 词条匹配一个期望值
"query": {
    "term": {
      "customer_last_name.keyword": {
        "value": "Smith"
      }
    }
}
// terms, 词条匹配多个期望值
"query": {
    "terms": {
      "customer_last_name.keyword": [
        "Smith",
        "Jensen"
      ]
    }
}
```
## 复合查询 bool
see [Boolean query DSL](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-bool-query.html)
```
// use bool query to find document matches all criterias
"query": {
    "bool": {
        "must_not": [
            {"term": {
                "currency": {
                "value": "USD"
                }
            }}
        ]
    }
}
```
## 其他查询
```
// range, query the document within the filed match the value range
// range can be lt, lte, gt, gte
"query": {
    "range": {
      "taxful_total_price": {
        "gt": 100,
        "lt": 200
      }
    }
}
// exists, the field must exist in document
"query": {
    "exists": {
      "field": "currency"
    }
}
```