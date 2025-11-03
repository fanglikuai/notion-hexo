---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665BXKYUKB%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T110045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGkeuUjD5MgFCn19HLS7830QBIB%2BQAkZd9qS5kjGvOCjAiB5tvsfjKRyRhlJpi6p9Vp4Fxj0W4mEJ9IFIjJFE6OCByr%2FAwhbEAAaDDYzNzQyMzE4MzgwNSIMBJ9PsYOoTFr0umcbKtwD1z3IuyVy%2F9PiPIy4%2BgJ%2FK1%2B%2B%2BtFeKeBslmsQ54%2FMRvgrtvx1YNnONycehAu0JhlyJSbIh6V17jWBveSJYTZt1nfoWEXl%2F3DGpFrAhas2tNL7IHqHNQfOOufkdxb0rGqMa2Pl7koJdXii8RTqvY%2BwsGqVV4zbDSpYFokTYva74qNuLRZSoYZxYcvEzSDQAl1zIndnSCZ%2Fe1gYrb6MEEeVFKovjraMrsGoI932GYBINp7ePBcEvEsGlkMOG%2BGmqidJkOzui6AqA5GwCuYEdA0gv%2FXQjwwGEv3rSj1DaU2TiaBbkjUJtpdnOYwm6vQ0F0q6zZIa%2FOFETquBFXR9uqRhQeJoMeVSplSUKc7%2FAgmW84pBlh0H9BOeX1yP3%2BxKNOF28XbdR2Y4D2h%2FAksC9v9CXzCAm6OnV0ibGP4xMlKmbjmZ%2BvA3egixcO4x1ZtEblG2jdadtI2cxdY3yuWv9wQLx%2BeCue673Z9SUncYk8nkeaaN%2BHoAtEXNN93PTOCiUlOBpm%2B8nWJmvdXS006hL%2BBBbIlkPpSuYUj1d4FUZFE1TRxOk4WSytEU4zelPZYMFoRIdylXjxgl%2FLVM3CZjDTZUhIgvcOjfFaZNwvaQn3SOBvx%2FdrsM%2FzGZCF8QOo8whPKhyAY6pgHkMM6CI5nhrlPpFynDa5w%2FNG2qHElWf5DtUfgmlwZCI9K%2BgLAXZRndGYcNOYNlCS7Nv9xE2PtIrmhPSUPI8NAegddvSPhZ27msEJB5SoDnxW5LQCUC8uYr%2Fg8%2BT79q%2B59OckmC%2Fqr3Wivhe6QLhjvdOcj12qVyHqmgyJSXTzIdDbjP4ic%2BU0fndLuGZq1DCMH9KLgfgxSjKq0cQh3AYMXblIPJQhNt&X-Amz-Signature=2a543e0af2b8be3a4b7f56df29ce47f0b06b91077aba19cf2cbd108728141d02&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

