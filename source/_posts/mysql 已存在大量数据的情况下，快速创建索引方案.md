---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHFZTHNP%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T170056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHDJWIP%2FHFoQbEOMk2pCCA%2F9MLG%2BG1aTe5bs13Lj9NNdAiBzkO7LRgkFBhxCaEe4sUHYyz27idt2EyYz5MvPqSz27ir%2FAwgyEAAaDDYzNzQyMzE4MzgwNSIM02oqGKtdhhhCSRjtKtwDR1n5faGop1k%2BjqnuIbhPbzGCHRvjtQ98m2yP6Rfelr3LYG%2BWi5BbVILhWj0CLRfC1pVICoFeThMBtjCN3aM3dA1%2FeJfp%2FtGfpIZz6BDLvnuJQsD2OOnE6ulPOGgFvN6fTTOXZc1cfzCRg3Fd8Tp2tSTqOEDln3v8K2OjwlxR0dQfY%2FOLwgIJneusuuAgleTafmWu1AIDylPuAr36HSgQvh0SJT8tekoFOrTRrXYWVbdDmt2V%2BXDwh6eOm5Fa%2Bw%2FlBR2thPzMZU4352inldlPNOTBzHqOuuz4dOdJexwAMbJC%2B73m9ClLEN8zXyVP%2B%2BNciAAxHb8xgOnw7pc4wiMoItQQuwEwBNTEwhpUrk4jlOopAQdUnz7rOf9HVB5XccgEcS8HHtf0w6SDxvIICCzzL0qrvzW2Rz6aUBZ4Yi59pBzHX2Tgv4Mdvj0bIHVhdgEtXFXosrI%2BhylrLNAuSs%2FkJB%2B5NddzFm%2BnE1iGB1ZDc5j50MlzlL%2FQJjzA7bx%2BdQ82GExRXPjzh5k4gHvUB9ieQs9bMnQaNYDTgieouS7BfCaFKKzNqylvW69VXJpRe%2Bu2iWt40gwT0ZMR474cCWjwSApahTbrGRuorkwLv%2BpCFW78nFC7M3sDCwAjs5gwxc76xgY6pgGVlCoQm6F0a%2FZleU2WsvTViOyEo6541nl4zqyckZe4%2BrdasTs07UnhFiPChy4hfNFVyeSNu1UgbUzSCV%2FAYiq%2FIHwLa6gEeA7sfSw2dcHt%2BG9FmO00ovFDi%2BmUMIVLGmrq2DzX0oxVoyxulbTGYyDZjeTxmA7eAUw367qa5TS6iLtO6sgDplbEnBddjQuJZyvi%2Fvjc6JQvRXUg5AFnJuEqUElpF42S&X-Amz-Signature=30ccabd9557d14e83c5a67da376c4b23e3982857c57f9d3d5da881beaf62840b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

