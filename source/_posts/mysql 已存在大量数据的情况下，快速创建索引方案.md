---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RHA6TGSC%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T060047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD46kQ5uY3ylUtJnEBoc9Lm0Jp3KldXtF8vp7M17nB8BgIgSaRcK6y5RIRJ7nHvLEH%2FosgLb5qy%2BaVsrrLDn2YQtv4q%2FwMIZxAAGgw2Mzc0MjMxODM4MDUiDBivjQGysjdy%2BOneqircA6thQ%2FElPG51NAZmregjhn2T6D%2Bl2gYpYFy1ri1yAYAkgLlKaMzl%2FIpWOPF6HoJNc9kb1qdASNBFdMl%2BOzJv9oK6iZebk7Jx7VyuoaBpiTHknc6hjfDSoMaLW489oMaEkefg99hZc6eZgH2qVfcLqsAJYceTVWIVOvKkhHN92wSEUF3IpzYYrfHr1NZzBpMHTLLw5dZiK7XCtHueKH4Q0BuwSOMHDZ0dc5zd5Tqi6lRGtwfabnjNWQpYeaItK8bQ1J3ZjInr1rosmCCViX9c7vr283GOhZjyEFUIAyX6ILMcQ3eVs8DlRrSEJXanDcU2milNaWkjzJ4Wgh7gyWjCztaSId3xmh5LoySBhTlSU%2FL36QNLL1W1sDmgO8W%2BJxeIviO0wNr6JlccZkoFsp%2BMnvA5yfJjg8K8pDmus2tL9HWmUo7M5mQxu8ZEa760VX4FGF649rHqJ0gEzbD0Xoer3aHkttDh7D6CN%2FEcvwEqJhMqUdrsw5lHsDFkaYwlTg9iI8BAigvLYqoxRFUdONlOAO9qNn0YrXdKzlrNR7IbWnxBsJ0izWepaPIia2uUHNS50IUsCzoRU7BRh3tSFLWTuEQuuQ0lNl7bdF2ZQtbHkjSDFK%2FJgCAeGJ4hYxuwMMmClckGOqUB82GqkG4uHppGvOFPclh%2F0KOxu0kaLMEu6n0ifnu4PJZgcOA%2BuLfBuqIbbHmvT2DB9HXfgC2AzqIdGKmaWkw9YUgifk6mIykPlioXXPI0G17Q0NFJlDVbTWaaH4wljpGEECHJAb2U8D4qdHu4jAIhHYOBtIhg4Ps7txQragsVhCMw1kl%2BTfNE%2FwQ2nT6wiSHJmiZECFWdDDBiNFL0ONBYsHQM4iT7&X-Amz-Signature=b09663a9e18ae2183bd1e2e7b4fa97dd5d68e12a19fbd99140b201630c04b381&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

