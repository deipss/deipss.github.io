---
layout: default
title: Pytest
parent: Python
---

# 文件加载

## path
```phthon
import json
import  os
import yaml
if __name__ == '__main__':
    print(__file__)
    relpath = os.path.relpath(__file__)
    print(os.path.split(relpath))
    print(relpath)
    print(os.getcwd())


    path = os.path.join('dir1', 'dir2', 'file.txt')
    print(path)
```

## json yaml
```python
import json
import  os
import yaml
if __name__ == '__main__':
    with open("demo.yaml", "r") as file:
        data = yaml.safe_load(file)
        print(data)
        print(data['json'])

    with open("demo.json", "r") as file:
        data = json.load(file)
        print(data['sites'])
```


# Pytest



# Allure


# Client


## mysql


## rocketmq


## redis


## es


# log


# python 部署

## 搜索可用的版本包
pip install requests==
