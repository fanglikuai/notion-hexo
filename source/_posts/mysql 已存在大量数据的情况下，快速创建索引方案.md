---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZS367O7I%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T170045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICoEEGppcG6plVGGgS5STKfZpmLhC7v1RT38nhDjpXQWAiBMShf6tGbZyza3QLfwcMIkoJdb44NJKsi90GOwH4MCoCqIBAiP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMMrnaceImw19HH7IfKtwDnwC0tv1KStQ3EQgi66%2BZvQDRHetNYjoCBySq0w4K8JCkEbyzBMcJ2x%2Fhr%2BpmMICAgI2o0l9qIbIq4Zo%2BOhCPL1c4yJBTR9sF1%2BECMTmt%2FsTpAG4tLgDDzrZkjX40b2RLi62hWL6pL%2FQNnkboR5b1F4mubTtKeAS7jGUScxwjBKZdrD1oyQGiWxKF%2BQDzHZn3g%2Bpy%2FX%2BWuLf8OqnlabC1Ceq5HYhg%2Fj47ZQGL5VAWiqcvecfiZEDfJnTkYsg3nU5KwES0YS%2BlhNGkq%2FoHywZV%2FbUgptV%2BgjLlVvIb0JTx8D%2FKdgofSs6bN5SIKBXPxS72BIa4qZQZO2hnMMINRyCsyvyKNirgZhdnzjy%2BnlDQ%2FEK49zswC3XPmSYCi4ygwBuGsMWDnfpuC9awi8PgEqwxW%2FVIwErNENm9t14enNP5qmSVBeMXFmlhaqCGFqecuLQQ3SsIEdFiSZNWTNJkYo%2BDhdOQru%2FwzpUsngq3QeQqNd5mvKSUDIIVpyK51tlV%2Fl%2FZEE70pIK2M4J8B%2FxHkO45JVZh9bXKmJPt0aXIhdziQcdwoPvFp9a%2F4KFaN%2FIORG37lC%2BRg2SEwWUeyVu7gwxLLRA%2BdZjUFk4SSxbb7%2FFzhS8AmN7vd%2BQO23spEOYwi9f4xwY6pgG%2Bf8MbrKj1D%2FVSpj0f%2BQpyiI7XQU1VI5aATzaX1iWwsm9rsSgNGDLoURNLN4KyRHuwgE4uXX8Hg2qT5ipfB%2BC2J0WZ3LJe2UlOPaEtqTZrbTOpXYHh6%2ByuBLleu2nyvtA0pEy10RUEks8ZaiujuozOB5OkOD91sFx71rltLZUZm9darL2MZXv7r1ZTFdHoITDyDkw5FfJQimTgH6iy%2FpabY2GKKpp5&X-Amz-Signature=ed8aa28d7dab81bb6fa4df53675774937923cb80f59e8f1516b597c241a3ed90&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

