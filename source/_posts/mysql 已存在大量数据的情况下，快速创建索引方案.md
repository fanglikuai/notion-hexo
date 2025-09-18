---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X7JJKXOB%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T190051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJHMEUCIChwcraQVZ0ojuyvhSRwEiEoAdzMH4dBdy9P95J%2FmtN1AiEAsrLVHOYxFUid2m2r4deBydHTri4QsxQtOy%2B0rKeuSuYqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDClW9bVLGD6OSSh4WCrcA7wWFuW%2Bye448ODb2nFLqE%2FYoz%2Fs6x9IOvQVmuNeaOI%2F%2BDmE3oizXfGt5sycloXyCNBki6Ow5EfQhIdUZyEb4RGpz63KwljDHDc5jFiVsYT70G%2BEacQ3%2Bov5PQegWOKi5%2B8XXdVNrBSdCqZxFS5SzpwMnY0F4X1E5WWwktRUCJPvYBrq5b8XnwPLjXJVTiPFEIUc4fa1sZNJ42R0TLFXMj5RTVWwas4f3ijDGKWNWkqLUHUipnakpaHroyGwlCv89WcPGa52uBCA0hxQiNrClNQk5tNH9V1Viz%2BYp848LZ8oJHqKXFw3pRypuRsohYbRE6BPn3F2NaOr97V%2BdSZ%2BRcFm3W87WKxVBpuz%2BpnPmRkwOT0Y2G4mpe2QVhLm9MW1RCUlN4zllkse1%2FeEKyFDYtRHAEqDLrsuuv5lazaihhPG2Ylj5DmEmNWiJTH7RmAv3LgAUkVuq9gBiz3vAyTPDGgLOa1PStXizIBSYdMROYNwKoVxrFXyNkonLf25iNjqbpnKpCKiKbdap1O2Qshe7dVql8q9WDMqZjJa4q8cQIpQ3pipoHed8zrJttLhpayt%2BLVJIUWHJ8N7NiIB%2BRa304990Lw2usFw6y7HfMZudCiDp4LhKDhEx4pnMLePML35sMYGOqUBxE27i7lcSItPPnbV02YJdrC6UBzbMKvvR9SWKUEKonoPC1H%2FoapGKCbXW3iXkWwq6Fj6qum0btfqtF8mSvipHorxiDvxWKei2rWFZqRa1Y32nGhB6fNLDXPOMyOQsIs0zq4DpXBM6kL7J4D89lYRYhXV%2BUT%2BbIPmWzfInzhNNOxjJpkcM%2B07nZrhy1SUOsb22T1h%2FabwefboLWkEQ2ZmJQjhXzkQ&X-Amz-Signature=abd999c053cfa0e8e4d31fa89d3f5774dc9d9565e7d49e6c41519056f1b58d4f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

