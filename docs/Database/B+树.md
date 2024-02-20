---
layout: default
title: B+树
parent: Database
nav_order: 8
---

# B+树
定义：
B+树是一种数据结构，是一个N叉排序树，每个节点通常有多个孩子，一棵B+树包含根节点、内部节点和叶子节点。
根节点可能是一个叶子节点， 也可能是一个包含两个或两个以上孩子节点的节点。

B+树通常用于数据库和操作系统的文件系统中。NTFS、ReiserFS、NSS、XFS、JFS、ReFS和BFS等文件系统都在使用B+树作为元数据索引。
B+树的特点是能够保持数据稳定有序， 其插入与修改拥有较稳定的对数时间复杂度。B+树元素自底向上插入。


# 参考文献

- https://ivanzz1001.github.io/records/post/data-structure/2018/06/16/ds-bplustree