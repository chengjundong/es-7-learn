# 下载ES和KIBANA
[ElasticSearch](https://hub.docker.com/_/elasticsearch?tab=description)  
[Kibana](https://hub.docker.com/_/kibana?tab=description)

# 从docker启动
```
// 创建一个network
docker network create es

// 创建ES
docker run -d --name elasticsearch --net es -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.8.1

// 创建KIBANA
docker run -d --name kibana --net es -p 5601:5601 kibana:7.8.1

// 启动
docker start elasticsearch kibana

// 停止
docker stop elasticsearch kibana
```