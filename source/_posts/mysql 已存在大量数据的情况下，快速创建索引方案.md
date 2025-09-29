---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666HKQBBLO%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T180108Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJGMEQCIHWkO1UlxR0Uzm4RoU1SEY3BOdzmktNBp3WNwzVZWRmxAiAzDNbF6peXjSnWLfqXPsnzmG%2B%2FAXgVjGNvysvR2G%2F%2FqiqIBAjZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMrIGVjzXRUSQUoQr7KtwDwIELMTPYpLVxWi1Ae5cPJsK8NYZwXdq8jOTOZMhtsLAGgj82HkNm8aHA8W3%2FKGVuBq6zmBmRZQi2B0etKrhzlAZwl2v0oYc2HcuGzMAv0KRjomn1NMQ4CjraKCqnx4QTp2yqZLdIGr0M9a0Y%2F1XmF2J4OF03hK7bOaw%2FuMGbaoP3%2FmFhbabkEqC42IOtyvtSJN6jLZEMgmXPBEFB%2ByZOq9GjPA6ghvRk2Z00Ajn5CrDHmL8xVK3EFWGcH8Le7220k8bhTlr6POBilXRgvrDnQO6eCwhP8MzkwdUo2fM5F1tlB6MmB945NZH8CTGs%2BMrnF5OarXmW2EVwMultYTPFLvJyIzaylBUM%2FMXml4uDa2rZQIbyjLNbkMAq3LuIsk7id4rFfN%2FLnBt4xWtzBGgGm8zVeaNDjxKJkAy3okfYaTRRkeqLQOKO%2FzhX%2F7W9z0xYbTxcxeckc5ZQ9T4mtmmO7QImLm2X4rDckdhV4Z5JSStCON2um1tGBNozcDlnG83ZnSP5OJeuDYWORXqS5VpR6bdFhLUqt83%2BOFFPvLP2BLVsLKsBSBidMvy2HiGJn%2F4cHKIs5%2Ba8UVpZQb2WPHdVEXEBe19CFSDXLSDF5k%2F3L3vc2dJKcImVRMKbslswwdTqxgY6pgFtxHuZ635w4U0gKmXsU8NV19Hz7yKtFV3s3Wc84gr4%2BvG1So%2BDbTwkTESEpFYH1%2FCUvcQ%2FCbiPcvR%2B%2BVgeVVgQap2jOEx%2FR9tBaiLAueQR0bO9LdIjGqAK2I2W1cgaSSVmfSWsT84hSong0EovzlnJU53SaTfWUgPfRJt8ASnXxd%2F%2BmXDuEpWPHzYthR4%2FijwvDJgMl%2Fx653L134LdzUFaMjmECuLS&X-Amz-Signature=f0b9b7b4aa958c06746f8c1352d0c6bbd62f033cc553d3a38f22f12327b7061c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

