---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662H4T5T7E%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T090059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDSTmXwilsqV1d9Cgl3gKJO3eKVIab9stTWwIkNE0YqxgIhANQ9FTMEEZVkrR%2F94ueiv7OHkmBGjH5lPFmLZxDko%2Fo2KogECLb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw%2BbzV2HJ8lmtSK6Wwq3ANoMukOErDlSgFvt%2Br%2FaryCd7NnR9Tg9ThtBW6WpJKi%2BgU6FD%2BMPHUOXP0vHU28hEI%2FiveC2O7UMcIriNgBzUPw6l%2FthwINZ0Gf1c9Jm80aKlSnHo9xcw5eBcVD%2F%2FLf7Jy8cyaPxIEIZAi%2F%2Fu2rlzALZyoG%2FD7EgcEZKnJJfyU4pjfnHgps0870cNETOYD7HqMAmjyUEDWQZ1%2BwmN2KtMSxXaxMLYemdV%2FdyLPTP%2BCUY3WHFB%2BJxYqIiM2Ojg2g7PHzArBBNjV0jLXFsY0q%2BYfPMKZpD3kB6LjQaM8Ek2Gb7pzybHr4CLraWr67BTbLo9rXWoFZgCDyYK06VrCF42kFFWC4S5hfqThIYb1RBd812leC4xzmFSmTiiIWAQ6hqIDWC7n4OM3Dv%2FfV%2BtSelknlEQlcJc7PZ%2F6X0qx5rml6Fninn3xPtDIb0LQar88%2BuM%2B6SfUcyMI4LA0%2BUIQCXjrvhxHu2dnxSQD1eFDdmJuWcDqBroinA1CpckBN949dnZMGNKUXOFCFO8UFRxOC4%2B7OKdpjfXXPGoo9BpqlNgJlNGiRf3pWiePQBiOx3nyyvcjLqFZUHdtysnRilgMcnQKgTDWO5JdQ2ZqY1CcVV3yeCDwpZLb9qpZCtgF1oDCY%2B7XIBjqkAXuc6KuDGWuggrh2zoYst7%2FzpN9YkdONCbwBHSwrGhKNj6Q42AISlxCbr3j%2FjyTylElrWUghHS9tqaTIJDHgoBlWyF1zytPx1KpXRQ4nwUTVhc0%2BFEBzN3GiNGYkxaI6CO5ABJzFqLba6HSc3ubLhnsU7k2SJ2S0CR7uMEhwUG8IXgjMTs1I3rwK6uVBgoXJht2JOcyKrfS7TYO1ZU%2Fri5y3lwie&X-Amz-Signature=8a3e34e12f94db37c954678d15514a7bbc3e13553eb5beeb126cba6ae6f76dec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

