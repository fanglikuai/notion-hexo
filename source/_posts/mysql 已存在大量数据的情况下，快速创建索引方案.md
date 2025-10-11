---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WWBJQPWG%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T060046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJHMEUCIGioOW%2B7SYIOFsbDp33M3hyvgqkffsNUnMrcAhsOiCAHAiEA2Bwh0PnQQIWQCpkcH0%2FQiCNk1fZtNlhc5OBAh0o9cdoqiAQI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDXlK8flbNfg9YwM3yrcA7ETZB1q2mTeNZ2vpavlx69Lbt4D2wI%2BApHgU3B%2Ffv8684Kd2H5Birw2PCuQ5TlYkOAWAlnt40FZBHVEVOz%2BCs8AB5jnDVaOaj8dBewoNuKQjB1tWHCpAqd2wx5QL3FjQGrXsC8H35leabYqpVzd3qtEUpA%2FggbXkv%2Bd1bBM77f%2F0HFzfyO0uxgAOnC1vfZzVAXXCPex4l0h1laWWHOnVNjQCkWM7jo40DsVIZfFGE3eImvvP2X7oWPTEw%2FAPQWLoMob0iSWYaoVF5PPvIpKhWvE3iZ4gEK2wIzCg2H8GOAhVpRsKtk12xD7r7vD1S6SQ6N0ZsNQSh1SIvG7yh8Yi8suu2G61KPOeXrx9bnV97WjfBh%2F3gFLnNrnc%2F3egjQ9iwBm22x8PpZloQoczCsDjaPxsiEEYkbGTjsmbgvMRrLzEFyzybenyqEoNa0daQztXsXEjU%2B1VNPPmQCWDXl%2BqQSYHC7nrql00o3oSxTw6Gu4HGlqxKkC7OqiRdR7%2F6%2BFfYLXSLw7JocSxJ3PCXVydZu%2B0a1oWE4W8wFVUc3Rwo6UWhe1c%2BBj9ujvapJK9KrHYrf7bH9yF0dVnII5FjodrvMe%2BwmeP4%2B%2F7RM4MhV9LupIbnaZYOWdddzf9ZS%2FMKPip8cGOqUBxcVuKsp3LVJPXuVUIceYLGGfhK0GO6q9XhnTW%2BwseqvJOliMOh82s1T2ba%2FW5RaEyBFiX95z7ssdF%2F6Mm4bT9TYueMv2P7h%2BIMgIaLIZAvv8X9vM7kzXbRq9wMTdtfBzwAB0mumR%2B8H63DFsWpbO6SNN3VDWcc3uUyUcrujWI%2B5aSxRaOoTYxxHeajkIkjTrjLOwPWQ2k6YKjqXDJXnPfb4tsxMg&X-Amz-Signature=b0ca7d43bc9b8f1105a79aa0ffdbd95aff8a1915450c0ea28fc62a8d5bfda7df&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

