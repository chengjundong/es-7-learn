# Discover
You should create an index pattern in KIBANA index management before using discover
## Basic
- search
- share link
- download report

## What you can do
- choose index pattern
- choose time filter at top-right
- filter the search result by using +/- under field
- add / remove fields in search result

## KQL
Introduced as experimental feature in 6.3, promoted to default feature in 7.0  
```
response: 200 // to find all doc within $.response = 200

message: "hello world" // phrase query

message: hello world // token query; analyzer will be used; to find

customer_first_name.keyword: Youssef* // wildcard search

customer_first_name.keyword: Youssef* AND category.keyword : Men* // combine multiple criterias

customer_id: (27 OR 52) // multiple values per field

NOT customer_id: (27 OR 52) // contrary criteria 

response:* // all document whose response exists

products.base_price > 100 // nested field
```

## Lucence query language (legacy)
Most them are similar like KQL
```
customer_id: [10 TO 50] // range search
```

## Save
1. save search -> share with link or CSV report
2. save query -> use at next time -> saved query can be updated / deleted as well

## View
1. single document
2. surrounding document, aka, context view

## field data statistics
click field name at left bar to see the data statistcis