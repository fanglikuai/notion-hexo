---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TJATL76J%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T060036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJHMEUCIF7SN%2F%2FTOGhQrFhSPU6f%2F%2BV14kdegzCCVbqxCyq%2Fm02cAiEAzpNoXy4SVkybzLOUlplsNos4WipbAfVOYWa2KvfwapMqiAQI5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEdFSuInq1rxyCJ1AyrcA0cAxKIATFsaQVKltVb1uLyObCzckGmgHBzv7VDSc8fdR8nEAJGrqDaUbKZQv7L88IjMulT%2BdHHiyyzEluI6xODQIPg%2FLmNTyQHcM9w2E8p7QMkUGt4L%2BihHs1QTmqSu1UXjV3AQoAspjVBtjBCaMmrfNSad32AYTU957A87Xv81%2FojgcY8zCUEQ5GQaxlE2DCi9YA0njGBIi0NVK62cmBOwZLffqQUVBT%2B3q8utxcWbqUnmIva99vQPvUxaUfGcmBKhM0M2vBan9D4%2BctsyreI4aK6jO%2FBSacGZLmhl4S8KnKrjgqEeAzwaSdMwzMDc7jexgopMOqXv%2FjvLmy%2B3NpdiwGQ%2F95S6qiw1keIDhSF8mxWCFQqcc9yhSSnEB0lNXO7UfAn0vHvI9KYrHLcyl04tnkdUc6WIfleuy6ekHcxWJ8NZKVsMvPypv70saQ9tr%2Faf7KP5g7cI1X%2B6B8%2FQd3hm6MvM2HFBC4IvMgAybdPLz4JOwubOtEEr9RQgmaTpemPNR5BMtxERwuA0zpJB6buFdqnQ6zo2nBxLphGXKNWYFMxu2n7YriYKRfzgSt81Kc7VSSUeRn4LkAX558xUjhGfQNh2%2FblDgzGh67HiTGl8E8JeTbZJgF%2Fc9JggMN7vi8gGOqUBOgm0NHsYsbju21RVrJHQRE23E6xFKLjO%2BxHts2rgL1KJash%2BmTqcpEAfnCVHdwbbaz75THxFFsH3xryGp4kWqaW1hwUd3X56t3NpzeHeAmSFXsqhbUglK6iwD1MnG3MQg5bacuIv7d51u0Gb8f6G6XnZXdTCaTtn9ODudCruMTunjvDYIRuQrYlB%2FB5tjEXUtsC%2BLV6ULQpCR5uXt8Q9LG%2Fyp1Qj&X-Amz-Signature=61d9895f4cbe3438885cf671ff1e6e16aad4fe180902cb49bea704ce902948fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

