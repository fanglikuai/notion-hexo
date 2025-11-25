---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662I4IZ3IF%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T170046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAyZmNJ%2Bwqo7Cd6GhEvI8EvGb7ZOXV8wcDyPxzIRXdw4AiEAuLMy8CcJl0JOZJjo20YL6%2Fa8u5owmdA1QZdLfVNT%2Flkq%2FwMIchAAGgw2Mzc0MjMxODM4MDUiDPjo6%2BJQCFVbzh2qECrcA874dUElUaeKjCAqNv%2FCTZgOGIzvtr52VHavvrsylsgNRmnvcWxiQBfHahOKShD1Zljdp9eqVlS%2F5lNtdmuShOa7%2F7yq92bmJm3Zwn3ndX1VIng8Nw6Kd5IrYuZCSY5Z9lx6Jp1qrMRjbvekmTBfGW2ZJRHla%2B4nXdV32Jyb9alD3lG1mjTlh3wWNL%2FSiyIFEOa933CSnU%2BZ9HreTgfbfvlFR3xW%2BgY4F4JwqTCy0wZvphs33NUvDTX%2B6gx5zut8BqgCTLnHe1ksaQTtu1SNbsFy4cbjI3hsCNyKFeU7hlRYkOJmfcVfEoD2ve8NyVHPLI5Hm%2F7b44xbNIEHqdS5uC89klLmJEP7mcXO3iSzLAUcK9DYh6u9EWMiRQZ0L8aFQFvFg3TaSAQ2HtCOV39MMgOyK89Z59PkrDf0Rfldu8wkLs%2Bsl8XB5hVhqZQy6nTi8Rt6I48Q%2FOfaY1frKIuZps2iuXL2pVrBPyP1uAYS52CZy%2F0CxMzg%2BV%2Br%2BkYvjMDy3nOC143%2Fh%2Bh%2BbvuWGQ%2BsDLwfCNKThYQfPB6XjBtJCNRX1BkaJnj1yhXxG0qMpCSsHI00tqqMyfYqWREHT%2Fiubi8uOzKFHgazDIOv52Gqr3Ug84Tc1DiEB83jXJmJMOe5l8kGOqUB%2BVLMgN5dkXNOHWPdhV7s1WZXEXZk789%2BlQTScohonItLtwTR4%2F06Kwr%2Fpey5y8iGoB1FQjs0g9qlEofmhBmGCtl0AzeqfmyqJ9BUZI3Q2J61x4YkI2TOxmVHFzmPUtPzBxRvEby445xOxSja5XpnPhwH8nEA%2F3aC0THp8Hz4HF2DS6O9dZOIRMBDO3fFiuniXBoCwR%2FFWSaJdcWWvtNrMggpydGr&X-Amz-Signature=c5e421ad5c4c9086eca3ea45da10f51ba2ff6a7db9fb6571cca6420d66af67ae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

