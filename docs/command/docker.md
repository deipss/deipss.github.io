---
layout: default
title: docker
parent: command
nav_order: 2
---

# 1. docker 常用命令

## 1.1. install docker on ubuntu

- https://docs.docker.com/engine/install/ubuntu/

## 1.2. install docker on centos

```shell
查看内核版本
uname -r
确保 yum 包更新到最新
sudo yum update
执行 docker 安装脚本
curl >fsSL [https://get.docker.com](https://get.docker.com) >o get>[docker.sh](http://docker.sh) sudo sh get>[docker.sh](http://docker.sh)
启动 Docker 进程
sudo systemctl start docker
开机启动
sudo systemctl enable docker 
```

- 镜像加速 新版的 Docker 使用 /etc/docker/daemon.json

```json
 {
  "registry-mirrors": [
    "http://hub-mirror.c.163.com"
  ]
}
```

## 1.3. docker 加速

- snap install docker 后镜像加速

> https://programlife.net/2020/09/12/ubuntu-snap-docker-registry-mirrors/

```shell
# 新版的ubuntu使用snap来管理一些软件，所以重启使用snap的命令
sudo systemctl list-units --type=service
sudo systemctl restart snap.docker.dockerd.service
docker info
```

## 1.4. 查询docker镜像

```shell
文档 https://hub.docker.com/
查看镜像  
docker image ls
docker images -a
查看已下载的 
docker images 
删除镜像 
docker imamges rm 
```

## 1.5. 容器启动

```bash
# 虚拟内存设置大一点
sysctl -w vm.max_map_count=262144

docker run --name tomcat -p 8080:8080 -d tomcat   --restart=always
docker run -p 27017:27017  -d mongo --restart=always
docker run -p 6379:6379  -d redis redis-server --appendonly yes --restart=always
# 自动启动
docker update --restart=always 01a07d12cfec
```

## 1.6. 删除未启动的容器

```bash
# 删除容器
docker rm $( docker ps -a -q)
# 删除镜像
docker rm $( docker images -a -q)
```

## 1.7. 查看端口映射

```shell
docker port [容器id]
```

## 1.8. 进行容器

```shell
docker exec -it [容器ID] /bin/bash

```

## 1.9. 日志查看

```shell
docker logs -f bf08b7f2cd89
```

## 1.10. 观测某个容器

```shell
docker inspect [容器ID]
```

# 2. 常见服务

## 2.1. mysql

```bash
docker run --name mysql_1 -p 3306:3306 -e MYSQL_ROOT_PASSWORD=deipss -d mysql:latest
docker exec -it mysql_1 bash
mysql -u root -p -h localhost
ALTER USER 'root'@'%' IDENTIFIED BY 'deipss' PASSWORD EXPIRE NEVER;
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'deipss'; 
FLUSH PRIVILEGES;
commit;
select Host,User,plugin from mysql.user;
```

- [https://www.cnblogs.com/limingxie/p/8655457.html](https://www.cnblogs.com/limingxie/p/8655457.html) 【使用docker运行mysql】
- [https://www.cnblogs.com/lifan1998/p/9177731.html](https://www.cnblogs.com/lifan1998/p/9177731.html) 【2059错误】
- [https://blog.csdn.net/ora_dy/article/details/80251487](https://blog.csdn.net/ora_dy/article/details/80251487)
  【2059错误】

## 2.2. zookeeper kakfa

```bash
docker pull wurstmeister/zookeeper  
docker pull wurstmeister/kafka  

docker run -d --name zookeeper -p 2181:2181 -t wurstmeister/zookeeper
docker run -d --name kafka -p 9092:9092 -e KAFKA_BROKER_ID=0 \
 -e KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181 --link zookeeper \
 -e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://192.168.0.101:9092 \
 -e KAFKA_LISTENERS=PLAINTEXT://0.0.0.0:9092 -t wurstmeister/kafka

KAFKA_ZOOKEEPER_CONNECT KAFKA_ADVERTISED_LISTENERS 
两个参数的192.168.204.128改为宿主机器的IP地址，如果不这么设置，
可能会导致在别的机器上访问不到kafka。

docker exec -it kafka /bin/bash

```

## 2.3. Mongo

```bash
docker run -d -p 27017:27017 -v mongo_configdb:/data/configdb -v mongo_db:/data/db --name mongo docker.io/mongo --auth
docker exec -it mongo mongo admin
db.createUser({ user: 'lutos', pwd: 'lutos', roles: [ { role: "userAdminAnyDatabase", db: "admin" } ] });
db.auth("lutos","lutos");

use lutos
db.createUser({ user: 'lutos', pwd: 'lutos', roles: [{ role: "readWrite", db: "lutos" }] });

docker exec -it mongo mongo -u lutos -p lutos lutos
```

## 2.4. RabbitMQ

```bash
docker pull rabbitmq:management
docker run -d -p 5672:5672 -p 15672:15672 --name myrabbitmq [id]
docker exec -it 9e83ee385ca7 /bin/bash 
rabbitmqctl  add_user  admin  admin
rabbitmqctl  set_user_tags admin administrator
```

## 2.5. elasticsearch

### 2.5.1. 8.0版本

- https://www.elastic.co/guide/en/kibana/current/docker.html
- https://levelup.gitconnected.com/docker-compose-made-easy-with-elasticsearch-and-kibana-4cb4110a80dd
- 还要安装kibana

```bash
If you want to set this permanently, you need to edit /etc/sysctl.conf and set vm.max_map_count to 262144.
# es 
docker network create elastic
docker pull docker.elastic.co/elasticsearch/elasticsearch:8.6.2
docker run --name es-node01 --net elastic -p 9200:9200 -p 9300:9300 -t docker.elastic.co/elasticsearch/elasticsearch:8.6.2 
# es密码设置
docker exec -it es-node01 /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
# token 重新获取
docker exec -it es-node01 /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana

# kibana 
docker pull docker.elastic.co/kibana/kibana:8.6.2
docker run --name kib-01 --net elastic -p 5601:5601 docker.elastic.co/kibana/kibana:8.6.2

```

### 2.5.2. 7.7版本

- https://www.elastic.co/guide/en/kibana/7.17/docker.html

```shell
docker network create elastic
docker pull docker.elastic.co/elasticsearch/elasticsearch:7.17.9
docker run --name es01-test --net elastic -p 127.0.0.1:9200:9200 -p 127.0.0.1:9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.17.9

docker pull docker.elastic.co/kibana/kibana:7.17.9
docker run --name kib01-test --net elastic -p 127.0.0.1:5601:5601 -e "ELASTICSEARCH_HOSTS=http://{本地ip}:9200" docker.elastic.co/kibana/kibana:7.17.9
```

## 2.6. redis

```bash
docker pull redis:latest
docker run -d --name redis -p 6379:6379 redis --requirepass "redis" --appendonly yes
```

## 2.7. rocketmq

https://hub.docker.com/r/xuchengen/rocketmq

```shell
# mq
docker pull xuchengen/rocketmq:latest
docker volume create rocketmq_data
# Linux 或 Mac
docker run -itd \
 --name=rocketmq \
 --hostname rocketmq \
 --restart=always \
 -p 8080:8080 \
 -p 9876:9876 \
 -p 10909:10909 \
 -p 10911:10911 \
 -p 10912:10912 \
 -v rocketmq_data:/home/app/data \
 -v /etc/localtime:/etc/localtime \
 -v /var/run/docker.sock:/var/run/docker.sock \
 --net=host \
 --restart=always \
 xuchengen/rocketmq:latest
```

## 2.8. zk

```shell
docker pull bitnami/zookeeper:latest

docker run --name=main-zk  --restart=always  -e ALLOW_ANONYMOUS_LOGIN=yes  -p 2181:2181  bitnami/zookeeper:latest 
```