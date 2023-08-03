---
layout: default
title: ElasticSearch
parent: Database
nav_order: 2
---

# ES集群常用命令

```shell
# 获取整个 cluster 的状态。
GET _cluster/state
# 集群健康状态 API
GET _cluster/health?pretty
# 索引信息 API
GET _cat/indices?pretty&v
# 节点状态 API
GET _nodes?pretty
# 主节点信息 API
GET _cat/master?pretty&v
# 分片分配、索引信息统计 API
GET _stats?pretty
# 节点状态信息统计 API
GET _nodes/stats?pretty

```

# 索引常用命令

## 查询

- GET /索引名/_doc/_search

```json
{
  "query": {
    "match": {
      "describe": "每天收益到账消息推送"
    }
  }
}
```

## 设置索引的配置

- PUT /索引名/_settings

```json
{
  "index": {
    "number_of_replicas": "0",
    "refresh_interval": "-1",
    "translog": {
      "sync_interval": "20s",
      "durability": "async",
      "flush_threshold_size": "1024mb"
    }
  }
}

```

### 关于索引的Translog

- https://www.elastic.co/guide/en/elasticsearch/reference/current/index-modules-translog.html

## 索引的mapping设置

PUT /my-index-000001/_mapping

```json
{
  "properties": {
    "email": {
      "type": "keyword",
      "index": false,
      "doc_values": false
    }
  }
}
```

# 文档操作

## 批量导入

POST my_goods/_bulk

```json
{
  "index": {
    "_id": 1
  }
}

```

## 删除某个文档，通过ID

- DELETE /my_goods/_doc/2

## 通过查询删除

POST /my_goods/_delete_by_query

```json
{
  "query": {
    "match": {
      "shopCode": "sc00002"
    }
  }
}
```

## 通过ID更新

POST /my_goods/_update/1

```json
{
  "doc": {
    "shopName": "张三店铺"
  }
}
```

## 判断id是否存在
- HEAD /my_goods/_doc/1