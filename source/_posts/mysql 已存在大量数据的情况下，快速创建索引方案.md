---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WEDQNQVQ%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T230041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBsuK%2BfsFPEdMqvfab16OQFGAHd1Dw0qRPKJHzw8ZHkBAiEA6aR8UwtIQNuGh02v%2FoDwVct%2B8GNsNYEAkyz3gHRYElcqiAQIx%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBgIg0%2FyaLX43n2vKyrcA2Qthmx8dBarDgV%2FLAEkgoCaAYRadqsYGvjyaSUF%2FtBG8Dcc58lx4rR8cef91OU5WzDeASIgSz0EdzDweXrF2NTmq9mgDV%2F7CRi%2FaoHLkm42iJL9QsB6Bebo%2FhQa0h6omkWuADLLI8bOYOz%2FE5j6dbcJ736R3H1cuf7wIudueAMsoabN8cUJwSj%2F5w9%2BtojqJ9qVd3oRm1mCwxKOxG4BH9CE3Snh46iTEfYlF1xC5ai77Q8rRGzipQN1eDlmyh0H31x8i6GwzPZ6mLOML1M9UMeBO3gjArIM2nZf4HGBCeNs0dN8PrFZ%2FKAEQtHNa6o%2F92SiW1X4%2Bq998FZ2jDWA8Ziemw%2BUN7RTLNtxfNkW7RsWhEAZB1I9ibd8xtW4VHDZHPf%2FmSW%2Bqw1VGtVUpvzjQeYDlKh6TnNR2SA1piO%2B4VZxhKZ%2F7%2F%2FTW7FqMBBZFIdrxIGgPI7s0izhMWtVph06osp3gz4jjNSufp4GPG6sZV%2Fasa6O4bezGyoKmfkB8F286%2BdCJFqiJbTUMdCwzF5LUgL8NtVmHcUpLvRd%2FIDPzfbAKmEOCQPSQTIxhVTJ4aN8Q1coZ9eLKHPNmIFfAOzEC3P96TAvhEA0PdYs42M3es98kqjr%2B8P7lKylxMUtMPHducgGOqUBqhQwCzwWEVA3OzVP69w%2BziTt66QOChbkVvgo290cFJQcBWlSbCZAj%2FXEgqYO9NcV1okSriK%2F6LkWrtB%2BsjUeP0zq%2BiVdwr0UyucAQPMBs%2FA6zm9hvpRYcMOp%2B85pcMZ0wteD5lDQUEG50p2EmKl5idpOUyoI62QxBY%2FU6vFoKyHJfKQfcN%2BR918Gq%2BkOhKIP1xkGuVFMd%2FFBs2IKN8XhN00iNNAR&X-Amz-Signature=54de8a3c3dc579bc889275c9e4df658088be8e9ae0193dfdb04332ac12207d3a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

