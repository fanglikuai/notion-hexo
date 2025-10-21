---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663QPREA7U%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T160048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGAaCXVzLXdlc3QtMiJIMEYCIQCn2qdos8vrKwJS48i0auyDltTFE%2FvqucL2cGnKFgMnUQIhANJlTMhlkGCuAilqzxcue3hBVoOsL%2FmZRb5Tj3e7pFmpKv8DCBkQABoMNjM3NDIzMTgzODA1IgwMEujScjVKC2sYMJ4q3AONXymAKiahWxvT2MOty%2FTwcgPS%2BM49tE9bDiITml%2BA5jTZGbxoxztOK3%2FMFj7s0vD%2BhqKSWLsKJhHhDIq6gonKqLEIboLFO5nBL6Cbi3LcSTMMD49qG0mI2BWlzlagW6afP5%2BceJwcAVGc66Oy2fc1TjVrB2yJzBvNiM%2Byg159JDK4ZTILTfSnVUMvN4GVWA5sP3GKfXJzysHWCGKQTAx3dMRCn6h%2BCJ0Uyk4Og8qeJ2drCWXyoXV2SL4wqBvj%2BuloUUUmuDO2PZQtGFZTz37fawz%2Bc7IAMwXATA46Z8o%2B84yWXG4f%2BG%2FF%2Bkt9axPLI7OQc7H6D%2FlQ0Q5jJciTVK70UHnKcF9lSku%2BzaeV2%2BgYFgst6anEHRbW93cOFrZsDSusTghYLnPPEzqZTT9HRFDHbpZTpLYVBou7C3SvzDm%2FeZ5OshIB8rkEx6r5yFosWngPVoTg26DIk4aoT480utxMXnKrmDg430p8rZIXM25J3Ifmox17JajOjYII6kR%2FHd0n%2FZpkZDBxgbfoxLwMcZeHnlBW1jACDbZaExfU%2BmqyqqTzLBNq%2Ben1t1C5x7Uu4VzA7TxE8%2F%2FLArLUmxs5gAhiR9Oj18nLrrlFK3nVcvnllZeoHLuQV9LnrWXMITD70N7HBjqkAep87lsCtnKihgzbauAEyFfuLiDomOIBHsUUkmKJ4TAoF658In4P%2BqTuIv7dSAeJqTrLRGURshkFzwXPbsAMl6iuDxD2l6m7Gg7iUfBwoUEvvj%2F8S7ic4veQcuW3odtYfsTSRHpWmgvpIcyAlAfopxWF7eVOYuLmhjI%2BHR9KfOTsrmE%2FRxPRpf%2F2%2FMJM5F97n2i80h73r7VIfzmYYsKy3asRxOb3&X-Amz-Signature=f165d6567ef54691f68aa5f8f48df7f0b71c0c526b950204fc5a16e7ab639868&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `create table text_assets like network_assets_blend`
4. 导入数据到临时表
5. 导入完成之后，将原来的表改为其他表名，作为备份，将原来的临时表改为真正的表名。

# 解决二（速度快，但是影响系统使用

1. 直接备份数据，导出sql文件，（这一步几分钟
2. 截断表（就是清空数据保留结构
3. 建立索引
4. **将sql文件中的删除表结构和新建表结构语句进行删除（重要）**
5. 导入sql备份文件

# 解决三（保守一点


就是方案2的改版，额外创建出一个临时表来存储数据。

