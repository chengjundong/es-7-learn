# KIBANA get started
## 导入测试数据
1. 主页
2. 选择 `Load a data set and a Kibana dashboard`
3. 导入3种测试数之一
4. 到dashboard查看数据

## 自建dashboard
### 创建index pattern
1. 主页 - Management
2. Kibana - Index Pattern
3. 创建index pattern；命名如，kibana_sample_data_ecommerce*
4. 选择一个date类型作为索引时间并完成

### 检查所有的index
`GET _cat/indices`

### Discover
使用KQL表达式：`customer_id >= 20 AND customer_gender: "MALE"`  
- 数字类型，customer_id
- TEXT类型，customer_gender

### 创建Map
1. visualize - map
2. 选择时间，time filter
3. add layer，选择Clusters and Grids
4. 选择index pattern，如kibana_sample_data_ecommerce
5. save layer，进入layer setting，可以调整颜色
6. 左上角选择save

# Setup KIBANA
1. kibana用NodeJS编写
2. kibana最好使用和ES相同的版本
3. 查看kibana的状态，`http://localhost:5601/status`