---
layout: default
title: npm
parent: Command
---

# npm使用国内镜像加速的几种方法原创
```shell
npm config set registry http://mirrors.cloud.tencent.com/npm/
npm config get registry
```


# 通过使用淘宝定制的cnpm安装

```shell
npm install -g cnpm --registry=https://registry.npmmirror.com
cnpm install xxx
```