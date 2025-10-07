---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V7B7ILP2%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T150105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA4aCXVzLXdlc3QtMiJHMEUCIB68Z9jlxH%2BnCbCoaEtBL2GctrKiDfumKUKh0%2Fqvd8PpAiEAvQi5vsCMM7H%2BotEYXHx8VNn67KfNBrynaeHIQR%2BbiI4qiAQIp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDn0EaKNOvkLOP7O4yrcA1rvA8OPzXm6ch3jWmgsNtAqzOsSfLFDl%2BmVIV4uIjRa1sEHszhi5Yskbho9XxxGpsI%2BCIFl%2FRcKnRC5wn2nX34O6I9OZeVfIdsGtyKbTa7t7gLvJLi5C0TYRUlfBrBJYOa6L6xaidXn3m1O0owBQBrv8FksAceuT2oJCCAmJWLxpzz8upUXdJQ%2B1qH4ZZqZhIFy9Pr%2BOop4T8C9rpihcyAbG2sPvCsNUn4tb0PusmpW3SEU65kBsjTvSkccauw2PsNHUXt4MZkSxyVMulHdeOOuqCPiTBStPKjoJrhVfhe2fcYzDKKuL4z%2BLK%2Bl1qWNDk7u2pTHHtWucW6U%2FXGVVuUdCp9lDKuHnaIUBtAaE72dfs2y1n%2BFPWiCB4cgFU96gcrG%2BJhFxPlf%2B4zPSJpDa23BZ0eyqvuqQ%2FdBfu8ZfFP%2FM2I5cDwtU5boZzSLJpivEvpi7Kysp6KTHFmimPHhpUsXwUya%2Fm6bsMN4BB2pVJI5CZBNyDZCoVyl5%2B5DWaIXp%2FkrFNmowPd4OqGVI5HpeBxImAGdxb8nrnJo2%2FeF046uJLYE8kp6S%2BbDuVXlksUQuySZ7bkBViwWKZSH1YnUPoYR4SPNvUEd8x%2FcWPHddKTKFE90HdnHMkCYlCjxMOe%2FlMcGOqUBbEmfdkD8pa1fJLBWq5ZLnojjGs%2Bd6OknhyiDYyZrJ2eRTVNJ9mIWidgyrES2RvBtoKXnk4MqDKadQsjXh2%2Fj83tKp6DG3jqrsWyEDMyXhQFpKX9sJan6z8tTWWPU0tU7mibcOsg95lsMpnYhQyWM11u05A23VOD71ZAkX0Xp9UITPp2i4e6oTATTrynY%2BvOhOOYbrCTrs8LlxenONg%2FZvxr08L5y&X-Amz-Signature=1c53b2788ea06f3d22ddd30959cf204895cac4bc3238bb22c58ad1e771a161c7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

