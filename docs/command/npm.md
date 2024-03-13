---
layout: default
title: npm
parent: Command
---

# 安装 node
```text
This package has installed:
	•	Node.js v21.6.2 to /usr/local/bin/node
	•	npm v10.2.4 to /usr/local/bin/npm
Make sure that /usr/local/bin is in your $PATH.
```

# npm使用国内镜像加速的几种方法
```shell
npm config set registry http://mirrors.cloud.tencent.com/npm/
npm config get registry
```


# 通过使用淘宝定制的cnpm安装

```shell
npm install -g cnpm --registry=https://registry.npmmirror.com
cnpm install xxx
```


# 应用启动
```shell
# 安装依赖
npm i
# 本地开发 启动项目
npm run serve
```