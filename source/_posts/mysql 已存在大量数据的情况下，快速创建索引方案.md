---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RUIFQYVB%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T070040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBYKV3zvaEzQT1lHKcaHaFXafDm5vKfiAWYcBB1iJ1SeAiEAg7oiMqw0GHsYY4QVC7EPjq8UuMfIz3hCR%2Fb65pUEazEqiAQIh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPvZD5oOu%2Bs1OsEU9ircAygD3%2BnEVORlh4wP4RE17evRzhCpufXJPsPDUDnscnKb1dPK97kPlR8ReM%2B0AXuVkmKo0Mgq3M%2FzihimxKNWZYJLbmQFpZo47H8tgfp8VtHGsjP8pEIZJxcaUYA9FbxRplC%2Frr9kZ%2FFAPEyBULsYsz8045f%2F3OLMXWx%2F%2BGiY2UXIqfEbmXJ3EH3zgiN8QqUY%2B1BLH%2By0CEHdKJslu0y3pschGmpO8%2BG2woHvDt2KJLFVvs8EmGrKNVUQRwMRMMtheLSZBhvB%2FS1oy1J5kk%2BGOpES6ZGIMXYp1zcNQYZBDTQEilJAEHzKMCCSfrZrqPS0qBwLZcxf2bnbD8cZOqyG9h1qC%2BeFc6Ox%2Fkf5%2BijkIRcIHOg%2BBjwiojzLfpSbN%2FUQ8CQLhaeVbg640stiENjH%2BDGXmIvo%2FrxV%2FzH6I9y7URt14G8rpUM4Yz7G3sKe5aynugGWSSQyzCip1y2OOqlccqQXoKPR0bKA4ZDoqW0ZUV5qfw2pv30Uhr5zRYm15cmzky7c0q0pvTqlmHWEXdwf4JfERHhA19n9hDnUm1COg3kWQIhXXouSopeaku6ttSNsVVCfvo3zj2d4zIcgcPJHx2YVzLfYJmTCreB7yRrAxnVNuFLOrKNMal9%2BTswsMLCNwscGOqUBm98vdtqBwS%2F3Ntsj8Hnkcv2EdHLAYiAyl%2FVeqW1GUFZp4gmeLwsR%2FaVrNd7SBlhYNiPmzhBPAasy7FLRmW578j2NC4bjcOCLOFGvVOMe1eXPL5S5py2s9n9Ccgj1v5XVCw5v3ZWIKI37dZ8TlfbEkt%2Ff%2B5nGix86BB79hW2xMtmAtTn%2FNu2zwuEVFxPaLqDii5MaexKkpx%2BGQ8iDYbdQ6nrO4W1Z&X-Amz-Signature=75775b5ac3c6ce7baf63994f45d54cb4609a4622e194f833e2ed0a7794001e42&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

