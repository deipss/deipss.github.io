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