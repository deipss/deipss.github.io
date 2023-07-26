---
layout: default
title: ElasticSearch实战手册阅读
parent: Database
nav_order: 6
---

# 部署优化
- 官方标准建议是：将 50％ 的可用内存（不超过 32 GB，一般建议最大设置为：31 GB）分配给 Elasticsearch 堆，而其余 50％ 留给 Lucene 缓存。
- 官方给出的合理的建议：每个分片数据大小：30GB-50GB。
- 默认情况下，Elasticsearch 文档每个字段都会被索引。如果某些字段不需要支持查询，可以在映射中配置 "index": false ，减少存储空间占用，并且提升写入速度。