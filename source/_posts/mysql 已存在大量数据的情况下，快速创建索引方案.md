---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TBP432YT%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T140054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGVOzIPiK1dKT5uKmxW%2FVLqt5K6S21uYrV6TK9LytExrAiEAmw41Q90ODh560GYPQBrymI9yueBkTjmyTWpXpyAViRUq%2FwMIdxAAGgw2Mzc0MjMxODM4MDUiDL%2F6sOaXllh%2FHpUSQyrcA%2BbsxEbzHryTyDIcfxy%2Fi%2Fu2J413Ka%2FAKgMCrqbwYg3X5uPZvTO2wiklwtTjjj5BAj74iSeiTa%2B5LLimbPBRS1LrV%2FgBYdIseWjmd7iRf2tkdYM8Ef5YlOoIZE6BnFBuE6PP3YpPIEGZlqDRpURSQrTiEfBi8X9vHcFj9jNW2j6VN3GhVQ6nKzFezwi2DwKx83D%2FdbqN85lJ0GFjyCJrX8gqG3uBZUcGJoVdKej7cidQEcuvR0LBNOcZh3tNnL3z6rHhzKrLfCURrAtDqM72wFDrwaEFn4YizdCgNjiE0D4x%2F81cwCsHMjsJdTQ0xsAgCsjD5CXzwvFyiYbZdMAPogRMV4JLqPWzllrafJAT%2B0G6NAATGCkL56vSPjxV6T7NnBBLfeV0gGvTIaGTQwaI1ZUTcoL%2Bvr5C8XodzsGkP7FgHr7tifiD5XiO8DkItt6m5GkiblBb1oIZF80cnjMFiYehKGXJkt015aucmMBlFZTeut13pGAGXd93kirAVm9uog8O9pVCzBFtjdyOM98C46XZmu2EyUOZ6OFvE%2B0MJxQBEp0DFtZt4bEW5%2FR6POsUNv%2BtMtqkr%2Buo%2F0DecBYGpiOrnrWASzWR8l9NdllTcqkdCmeQK8%2FhfgzHXPZsMK%2BK1cYGOqUBWuPJx8n5U9gjSP93f7N5e5Zwhq3rFzZTk1gsLGwx7%2BH5lDnJlYePwcgS%2Ft26oW1TWYv%2FAVdH9FGw2E4e1EBSzyFmM7jYXb90KH1R7bMoGnLZBkKlLs2qXKquJ%2ButHe61PXnUYSQUatJBiGcTXOJYoZLE0YAVTsyQRhHNuzwpL6uNwwvFbZ%2BuG0xqYOYtTPO0mD6etzKg0xV9P%2B0rKl9tbknWb%2BOo&X-Amz-Signature=96f2b6b4591b3cb3e0d2f33a58e9dbd6a8d8ebd799fda0e38a1294cae2039d02&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

