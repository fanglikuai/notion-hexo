---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SBOKEQ5B%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJIMEYCIQDJYsfgoL1sPnRa2Ks7J90TqWSYOm0cV8RXg4vgJYJKbwIhAOgw5h0cNhUDSgZUHZlH3%2FUjneWGehu7GyJbhd656iMCKv8DCEAQABoMNjM3NDIzMTgzODA1IgxDBrMGS%2FLGdBnrQwYq3ANaYfhTh0hlEbOCZw8e0mq07RBjwaOLM9YjWI6GBC50YAjuqeFTcQK2rKDLqfShxuaZDkrYrX3SXH7uLSkkN52Acz1fRxxX%2BFzexzB2DBmEMCWdiFxeB5YzyxjpEchFX%2B3fVq9Ne2okgFV1P3DC4QsDmI7vu4%2Bk0o4f1CzkYqCeQhJcxA5omY6aGB%2FwPihQh%2FLstLVAtZ79%2F74uNx0ITcMTs3lRMrbvogsiFw5tlgtC03%2FU7cMieVuhHhes6iSGjUJ%2BEU6lpqtamMO3jnnAMdY3UqCsnlKfILxLduLXxro4jwQ%2B8Pb3bG%2B%2B8Sfu5dFDoen0I5d9l8WVyGGUxgWR9g6Nx0nH%2BGkUA%2BgH37l3nI42NPjvm2dlQOWTksbt6%2B8GPnBDO7vy3fGzMsNAHdBRGDLrgVUSZBSAonFj8GPZxl2Xt9%2FdXJzIHm8kZ91hNccT%2BpndyUfclkMdEwjGJAHrxH6JEn4LB8ksVQAqRDqqG9wi1M3P5ablyMSdx7WL9e73HakqDrql3Icva5Xnp%2Bve%2BjpcuNb1P%2FiFdyJQiKBdpDUcou3C4%2BhcV9ehqhT7yar8bsNjp4%2FIPh9MF%2BG%2F4croD1f7G5kKP8m0dEPvxTYLrO7fl3vdPowLT9u5ijnhBDCOnNTIBjqkASfGhgX4CSpf%2BPZX7RiH1%2FTCPqkMiVydA2%2FXGMAWtME3XEINs63nGC7bYJJWjeBwvbKrTx9ezfgIgpPFwtTKKpqc8GwLOGs0RkOyMAvPv%2FRm%2BWZydIPdkeav6jZ2b8Oj6NX6hKeUO%2Bj%2BnWDWXPyLmz1gyXK%2FEE%2BlBtrimPBaFvV8T%2BHMC8qFf7YP1tSY6PZcVZaawzrrZzxqVS28c3AB534Ndc48&X-Amz-Signature=e0ca7dc5dfc646fcfeb71596d85ef7146bffe4d7fcfe704d344509ee542201bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

