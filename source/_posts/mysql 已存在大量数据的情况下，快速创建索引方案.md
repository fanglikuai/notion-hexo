---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667BPU7FXB%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T030234Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDNiWTuOdNc3kmHRsvBcvTO8XBS6GAEsPRK12ykvEMbSQIgNJyIcrPiPt4C7%2BEIIDxIw3GqUK5GvdCnULdSPVTN00EqiAQItP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJ800jYwqoNzm3g7RircA7%2Fp0V0zAf4tZX8tpDzD29F2NRzPdY4GRS0icig2OJIKVZGR9bj3YANtf2mkAecAN2dpbgPqki2jOKgZgOHldzmF2zY7izPHM%2FK49ZttqaY%2FmMKPyLomRN8NI%2B9%2BbARWXXPUHVE1DtGXeOt691K45nsREUoeFFQ7GCyuR%2BN6yV1lM%2FZI0mcCmQRObjwXjudC46iwpMdvjDRHpQjEhMnZL3YyMr7h5gKB3Qf%2FldC1nmGwxvTYy%2F7K%2Fe72K0Ni51Rs%2BYHahO8tW%2B%2BcJRWTSOhsxM4FGK2AojzcQaghaOwixt0AreSbWm8zQV%2BVkeGZi2bckzpUT3Gm9NhI9dCz%2BoJPzkdEVdQKzdDv8Qxy5UAdtjRUZlmw0utJudGyexs%2FbLnW3pQfKzpLJ2ljl6zkkwPsJpf111eHOYAUpNZZih%2B0B7jBh5UlQneJCWZigtFdEX7wmjZWzhJpsgiV6y15gVp5QDYH8B8cDQfNaCGzbXF7wkVWLuqPuvEblo0g6%2BIceyvBmZKrQvYlOqyJrH12w0ligWjkNh0DdFMUG9VSs%2BCGZxtNOQBmFJ0TnGS8TWCapdit4b0GG9vlq0Pwx5lK%2BeN2cM2U%2FSbo0E1w46GKEw0cBAEKv2L1z4vRdbQtPrOXMLy%2BtcgGOqUBsuR2LKLvWB3Bxi9cIxQzOOHrpcceDBp3PoONK4zI6bzeVhFxFtOZbXUsitKsTDamq7gHoqM2W5LHrNQfrAwk8RIRxMpeAxVkb40Fn0M2xZFbeEC74HYQ7yWRRcU2LqJz2H3xc3PCV8PJ8ewJVS4mZ5sPVXlSlrWQeuEGxZ%2FXAr3JJtaCpPELN%2BTKDbRtTB5GtLbRQL3qj9dhJDfY2Zf5jC1p4IF8&X-Amz-Signature=239a920f467740aaf0183b003fc78b7605b8525f9407ff09d4c98e070d0a3c73&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

