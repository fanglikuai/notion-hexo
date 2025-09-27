---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZRYLQQDZ%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T100055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIQC4kdRGJTO0rWPP4O2KtukQRphmFMgEo0zBjCkJWOC9zQIgNjFwzbt7u5vZKNZZVVmUjTEf9dtf6YSSxZm%2FSy%2BfAaEqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLc5cN2iVUAtddmqUCrcA09bSyFqf0f6jxOuOq%2FDUp1vxlxAvGuJDP9Iur757eohKQafH6tYd9kp0vGxyiCcPuRuihixzVk2c7GBM9wrASzNqyZaDpYRyEl1j3DxfiUc%2FJ93THnt2YSdcnicZJC2gCVqySQPbHswmvVGXlMR7zM5b5Xl874MkEfRPbyJOmf07BcmuhYO3ZdbZ5E98cbL%2BE%2FDPgeepmxoS%2Flz6e1PZhKyXtXcXpEqIi02wKUN9yslfBwaS8NIpZwbztEdS7So1VvTBpETUgfdiUwhr%2FlCGN2rx7MDdjXdK7MScMjNyJkraP5%2BdBkx%2FHaeIrQfni0RFwZHQIua%2FH4kln61VbXAKRvjH2pGfXaFZuT7tLWxx4EdiSGS8H33oHlEDug28aqPghFqwtkbaK3ZEv2HAU7bfDhuUjkkPIZPupurx%2Bui6MLILXRuF%2FwaFXlG4SmMlDwOa9tYNYEQfbIFnEVwv6WQFF7zJcYfv3%2BNg6eZWX%2Fw5fBYLulF2FZhfC1GH5%2BoulqrpnFn31NQS%2FlNaQJ1YEUb5x47S3cdxIynYHg3MQ3HtqFdPDTWaAiiQtWbxok%2FZsfrM1fUkZYTrKtMA7AMmzlMViBxei2CaY%2F%2FlJ8hungKGpM%2BcVOjLe2aLQxRcIScMLfi3sYGOqUBGHLWMgC9b6McP%2BH2Iep6R%2Fa5g2GF0zyCArEU7uIOBeq%2B2mMjcAUT%2Fo3ZK83B6GVBngsmo7EfcJe3iYczDuBchZ7LaUzI1Oe1kQujXfWHjU%2BCenUc9v44MEHfnRS1jfJ24ICqAFNvKjBo3tOE8RJJdPaKcn214xhoMt4RxqfyRu6VajhVEVkjvYoL8shBG70OH5SoCkv7IbpSrNi4hhrN%2FTCZmYF6&X-Amz-Signature=758b3d25fcf2042bd5d791c5e098ea72b1abed0c8508750542af3aa4431ed199&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

