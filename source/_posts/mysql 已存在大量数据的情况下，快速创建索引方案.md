---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XGSQNVPA%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T220047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHjxG6O%2BGuOoxaQy6rqN1VvbDjjkZu%2B6y2tityTQfaPrAiEA3NQdVgNEiUHOUsxRB62N7BVnAKCvkwnWVMp%2FTUXlYFIq%2FwMITxAAGgw2Mzc0MjMxODM4MDUiDOlMsv2V7rmYcWyQXSrcAzEycMsDsjeaWEqoiwOCYqfTefdoYMV%2BIjFoMqWSTjooTPWXm7l%2F%2FpQTCkv4d%2FrCt%2FpJ4dNcEsZtXAM%2BMEY4tIwniJsJugWZD8niXAwwDULd%2BJEwJ8SE0jrxGCSfEYGhWvMsa9ANFopW5WZ426zlicm9u3VWeVdlDFG7y3nCYckB8EoQA%2F6TmvdxFb2Y%2BKfdtOWj1%2BwmqZtz%2FiXi%2FEWRzIqUTErxUV9uhn0LKegyHov8C4moIIlyzuiIRiBL1GqKuf85qBdemYbxZaGhaHgsIFKxIeqNWAV7vBy1JedfhQGd9vWViCH%2FZRzNDPP8RvH8gs4MCwupS%2Fu8zO%2Bl5cs1veghgb%2FULlLqkwYZPkHmuKkiTOWu3DR%2BDaTM9WOTyzlocfkNaBg13mADonYgVa8gykx6XKHMGqcoRrQgZRluQ6B0fg9AqGNURPiO4lh0ksg41wMTl2zgH%2F1D1ovsMtbkvWGTOj5QbVPUVFk6kErI%2BQUfjBlF14xFmTWprIrklqIOJRiZkE4paQ8f4vgCRuQ8TKsjHItYqViZwIlu1NMc7sbdRqEolyZTf9pFEIzxph9q7CARlpg58I5zteq3s%2BAzkSrKwyTdtRfQi4Q9GnEOhVjVtl43zvYedTWYgK%2BZMPXI6scGOqUBz5CF1lcvgQmQaHCN5dcs%2FiR6cuEJa5hxkY3yLbYPL9vwfrZTpoZNcJunsyXaDAhK3TXUU4lmaXTLrDExJYmM5G3v7EdppxRZM4QmNOS%2FjwvY7TgeKp2gjaITgS9UsQLvMSi%2F6DiMz%2BNDizDwdgGlVdt9q4DWNFUi55YOvm8Gr1oV8kddmvEEYs%2F6uNBGnqWB9gZinldBE5mp%2FFiq%2BuvaJleq6lm9&X-Amz-Signature=64dad309a6c31e7232807553f39f35f30a334d2041e48a9c610e0420f3023251&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

