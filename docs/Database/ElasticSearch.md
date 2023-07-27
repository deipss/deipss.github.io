---
layout: default
title: ElasticSearch
parent: Database
nav_order: 2
---

# Beats 在 Elastic Stack 中的作用

# ES 常用命令

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


##  查询
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
