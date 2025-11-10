---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V46OXVUA%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T170048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJHMEUCIQC9RjdydfdzBdIu%2B%2FnK4CRmoC6SgbvVadDfOywE%2Bx00YAIgY%2FB%2FuKOol8h3RdOJdo6NgTRNdV5Ztt4y7d4awnbv5Acq%2FwMIChAAGgw2Mzc0MjMxODM4MDUiDNRFDuCaOQcE2OBBgSrcA1JW5wk3e3CFbnhP31ppD96v0yPyf47zZX%2Bg8rKjAJpqWBpHakZO%2BECbarA%2FjYbvIKBUrrF8Eii8gk6%2FbiyWkP%2FR8lXLK%2BPPgMMv%2FO4wM6mA40%2Bz1LfZE5NaFsR6pxH2%2Fvu1aJZ0hCHZiiPkxbz%2FLYLCdyFDRh2AqBejiLU0qmk0T5WNOFqWx8vAptFgYVfEJ5LpL9sohS1q4UfKskrDz7O4%2FKGEDlqyObh5AWf7D%2FqMTNl7Tsb6gsREI4DcsthWMvTaSb5A4rCbqoVOeVxgLsJ4MB39PyMLaWgSig9qBN4wOGBBUlRBP8Re1I807C9Vrbt%2FFdwtiitYc6spf%2BFJwI0f4LVu7MeJGmkUd0zpWuJr93wJ6JKlKIJ89VrsSKUccNyfhlGIBoA4qww%2F8YVyAhW2E3GHpzi7I08lRhpQ6JnxKqbqpBLVphMu2I3%2B%2BvyheCH5y%2F0gYYB%2FC2BVVMlqLquACCBNw6H7BkR3H2Uns%2F%2FsP0g6oGIVrNP2ujXSelacvT37i7j3KorTYoiCeZAzdBYG%2BGzWYLQeB58hdMijUUD30n0H4%2FKOVQGv4v7YTGbQ9m9GRCtYizbBrA6ku3cBCEjSuiomfQfq0dcePyFaWWY%2BQ7GdWAb4Epx%2FlJOTMK%2BryMgGOqUBOtSocY5tqQEw5u84JOJhRtQhsuB5Dmq5w%2B6MP%2BmF6HbvMw%2FQ%2BgJSqosaN6p5IF45fPPQMivpvv5YEuQjbhkxEC6uOfuBtg6ee68Yo%2Bd9EYM9hRK83noNn56kqTzfxJfCOWCByRy%2FuBWBTH%2BvHIV3sjdudv9Vmd5WiKi0NqtK%2FMOwEkS3RyHLF7g%2BgE6fIS3fSOOwPffnD%2BbvznjABgJHiQEwLFxW&X-Amz-Signature=2b5a5efc5d370be1cc2106ae9e8faf80d59c6eb234a30786450cf276a9cc7766&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

