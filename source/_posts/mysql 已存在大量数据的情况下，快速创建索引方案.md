---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667ZF6346%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T000049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJGMEQCIHo01tbP0qIplcDAlwVcoolzCFIuugzTK5C%2BBOCRkunFAiBDnbPjaKCap0RfxeD7EGPaG6K%2BjbrWj0v9wDEvrYwvCCqIBAj5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdsrnvyB7m4dZNJhVKtwD0HIT7qFULfICtocFzT3QT6vprfxg96F%2BWBL8HyBy6uVNgpI9nHVkvj7ENkD6LRuIFxqiXOrXw%2FioFkYLtThUlKPQoSIbbNkIzeQ5d95rqhjNp41FQPqnemR60Oi7tZmf1R1bdkBA%2BEjfRIdv9WJXdKFGCo9pI3N843vnGz%2Frauy8xjsBs3tR7GSMy%2BStWRyXSXyngdRdLcUqc%2FrgRZWbyMqUtvqxSZhWkNzEDFCLRwiqo78HQ85UTaCSGDb6p9K0DETXChKiT5%2FG%2FJ9SUTzp%2FoTLqQmmIxd0Zk6z0hIK541xIy%2BGD%2F%2BgYx5ZFbYIQkhfyUDAHHfv7vX%2BXOZdcSFhZIy6X1f8HEyMaskjqWmF9iNz2ystuN28GtXFM5mkvYHVPq%2FXW%2F5k5piqpgUlYrsDy9ctEjS4eRoFWcABrckphz2kdB2m5l2rvM5JNUeBbMfeCpgdoD9Vvr58Aj6NHxVZyyoO6EGE5Tq5J0M7J4or5VSEJBDJS2vOBGBAQ7OohA7AiyMGZQdzLHDoN4bzPBfbvACR6LBJv8duUuxWcfOPNDZKoUJamZb3LLUdz1oydlcorZrBUlxp1g7VnzOJKskmKhwrL5dlqsMi3wKBEYYSI0A8r1XroXmD4PfhIrAwgpfbxwY6pgEh16LwRNCnz4Vkt9XbsPTTEo6eCY7%2Fi5E3E4yLT%2FBu5EHQy6N3vKnZa6ypsSRFm4SpeoDIFZyFm18BAyzFpiDukh1p1cR3a8kbicgdv92e0Uqn%2B8ozO4sVwTwPdYzhXba4D%2BSMObgH7dIcDxrwSovjg77NNLwoPvDRFMj%2FuEbGNbccvPSCk7c3e%2BGsRKyRUAFRLeTkoj%2Bmob73Gh11nY9ahS7KYNKR&X-Amz-Signature=d3f6cbb1c576cb3ad91bb1c1adcaaaa4171c06d52352c041db5cf77a8afb7d65&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

