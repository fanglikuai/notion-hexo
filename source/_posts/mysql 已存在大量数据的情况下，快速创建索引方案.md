---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XIFVLTBD%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T220041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHRM2FZK7moBQJXSwX2%2BTp4IfPDkig3dm8trRtMhWyMwAiAGmJoxJ9R%2B1%2FG8suTw6Lu1SxiyVm67enoTLm0kFO56yyqIBAiX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMKH5IAgDjKsa3kXr2KtwDaQQLh1RzUlIz7nU8gtGZm%2BNzt89gPTA8GxPhII0q6CpirCRsNl%2BCgNz1nDFrKAwnZT6YHEIPpOr42BXlroFV1Hha2NOjL67RowjxVb60NFkIogwKEsxmhJ7Q3kDeCMa0629BOCcULxz3eomvn%2FGvek0LtJTXJwfEY1WG3cjJCJ3aXdfWCvGZ7aS5TS7dhVhR1Bm8HtR%2F08PzWMX7lf49sSIAmBaJP5hqpXYO47zhE5PGqf6rSINNhfFbV3Z2divFsgB9GW6uVIoqnRb5Cxi5lUsLHZf4nkXgeqqW68y3tzarODd%2FT112FIeThZj59PZV5KMWhu9Roe5ndCybgUjAzOfqyVlDbfk98QvNdGRcLg1kRVZJFxFYK7mB%2FLHfiQ%2B60KOlfPKSnzNy%2BMn6HNXkJEdj%2BrRyjOqkiZCdj%2BhQQBE%2Fs1Zylj%2FWGqBy%2FoM6sIiGryJHJFNwnnJ%2B3THyeOyKGBpdH%2FZtxbkMwrwOyRumL1q6v%2BIndap2f6oFFF7nNK%2BNnXDVouxbo1107hQih56jz13cD5rQvs56y%2FmlbmcRXJPhMCGCq06IBQjOwhEmcS4k84cQdrdollGw5yXlkZNZALUJw37XLq3DNr79EwcYq9BR1hhZQ%2FtFn%2B49ZbowpYyvyAY6pgHbZKduwv0cAzqgt5nSu4mmusmg7DLdLnQxRZc8S75cFR4LHECy47xnS0y%2BPzQXN5AwURVnluvV6HQS4lylscDB6sCX8oooarYho5RboX5nonGvhqb1L2km8Qjf2vdUCxLGaCPGlnAEY2dw%2FKf6PI1iXjKL2TSNui6VXmavvGRO4ZwaQ3mqkoZ%2Fze6Zs%2BENYHkc8OqKxcxFUDRMLMMULHa8BK5yub5Q&X-Amz-Signature=25a258c0bace5af299bab8c1658e9eb077e0ac7dde9fa06dd9fef93a69279eed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

