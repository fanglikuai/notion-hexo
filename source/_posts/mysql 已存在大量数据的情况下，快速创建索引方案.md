---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QGYHCXCO%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T200043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJHMEUCIQDT5FPE22QYV%2B4fDZ%2FNNyh8xVp1GcwkDrjFwKXFmiIq4AIgFtswhwL%2Bd3vtuL%2Bd%2Fm5VuhLgifZI7SoULVaZV8qSsdQqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDNdsmnvrQMoSQzMGircA5JaXTUOPSl%2FRfjwPZFk5IFEhFZe6zm2e9%2Ba1e8MqPAw%2FQMH17F8tmSJAlukjhSgm4lLLw6cDm070zGIHifMHVBBcP%2BZUasTnq8chdoxuM0pdDYmNh43VKsq%2B%2FntFplNp3RcaSLJXLZNtOANchqanXIZAfrsPASsdDidY%2Fifv5crx20qkIAW6wUr0K%2FUZezcnzlYNbySMISz1X2QaoZa23%2Ba6PNAlpbDHZ8V%2F9iPv8A8drD5C2wU6YzBGf1K0lMsomCaAMcu0nO4bebuUl2%2F4%2FlY4Stkj8Ik1Aw0rSgYedI9Dhlh%2F4Zo2pFJ%2B2NfsadSbdAI1L1WnqE1YH%2BkzLlpo6KGwSdKY4DMuUV6zutcWcM7DRDXP91bROO7JWYmbz2OL5izvdfQI0riXrJ5%2B4okR9ZKCFTpoQPISw8ANrem4j5JB1wAvnipQwT3fbi6wzhVK8e17XWNzVUfJBdup%2Bgu4M%2Bho3uuJ4UsuacSuqShyzBgOz3WUEx%2FaYDH6PGExSfWb6iGCDsQGQVR2LF6JZ4R%2FI1x1meBoGb5b%2B6JmHAwyxqFJJZPvQlg9VKeX7m4sI3AGHel7bwLXrxuobZxxXLcSaDyK7H0CoIlCr49w7dA4%2F9h83u%2Ffu8pRcklCHcYMNeAw8gGOqUB4wH9L5bHymILTWiAiWBnbPJxBAVFjldzW%2FsGbLkeL1BvQWce%2Frra4PcbHuBIzGZ2d2OBjwdG4h%2F2YmEwxs%2F%2F6n7CXd7X5AtSsX75bFrDfKbM6rlJvXed5BaeM2bWC7dz4B2JqJpn%2B7sRD8oUTVIsWUgnW4NpvZQ8nCRzuZEMaY6odUbfZuB2d5nXrVwGLIvOztODo%2B7sJud%2BTFoOUlC3A3c0Kp3X&X-Amz-Signature=b45813382b688190ced048860845ab331fa456dd7817c345c63b21b7b9578fd0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

