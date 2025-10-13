---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSOLUKZX%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T120048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC9xpGBJwolkcupLCxomIPfb%2FR1tyB6W0rKxjN%2FfxdbBwIgaVHNTI5fi6WodAbmJydfCUh%2F0h1DGd%2Bp%2BO7yS5TGO%2FYq%2FwMIRRAAGgw2Mzc0MjMxODM4MDUiDAUexFlmW5CfJ7x48SrcA09P7sSXOXz8tZ%2FfjWYG63%2Bdnm8lpWTIjk%2Bo0P3DegjDpSmJ%2FohIinnWHtnsJaoBRV3WMKoZodGYwcKk5g6M5xk%2B4XscH7VNvM9VYgSTlvfkzDEINdrpYzpFv1QhvZ0GdH4sRSqtDlDNj7tmeGfGJxIhVyuBeTVoSTas3yCSM2J0FJJCNOhliriCtd7PZgb1%2FSHdI0nfF%2BGbFtezY91dk8SWy2ng76qeqDeusLmwM3QaIrq%2Bkaql88vZc2JNMitZMs5%2BS5YqQ75EehH%2BztjzXjCp5rXpjTO0PZV5P7EqBkfNl2LfVmpWZ7YV3DdVPi7EJYG5K9ST1NkqdYPjjgjyfHtQxPLuEUwlQ2XcZnrzSXVpJFO8UhwFIkZBXslVm7vy7rVkdgZG5%2FzhbMGRqzlSehc3QQxvv7ADa2AxolRgcHghqWxvo9fw15VMLmkOJNN%2ByoAXaCb1l393LOU1Iac4yH5V9c%2B1%2B4gnCyiKxYmONGsrDwB7ze5SJTaAFgjZgPqQ0Tj8YsIvyUs43%2FFP75lfzw6g1K5hjFgxXW%2FMnR%2FyMTyLz9reZCy0c9CUeP8Iq1BD6AeihKxjCZ6v4VVq8%2BAMn62Lv%2BT%2B9yF6vSAVTkGLSlQ5lEMR%2B%2FGVRiyPmR8GMO3Ps8cGOqUBRdV%2F%2F2mwX4OFJrAnORxLCkAz8wX0urpBlXEmQhkOOA%2FdBqZVtPflH4khxV%2FYe89rJeIQiQesw6%2FtFFdPi2NMZWUUUQiksdamvaBUNtxMIRgZcWgSj6FRES7eoyGb1AE74sSz8S2Vls4KNhydvWpa6Tcr7qDpsOn0g9Ix%2BPQeZunuzdTBMnjg%2F1Yb5CJBBjDzxveOiQvyeLuOMu4JnojmqtsnbXlD&X-Amz-Signature=6a245e0c449c1887a5c90a3644363420e257969cbce3b2bc8d46cb35db16ce31&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

