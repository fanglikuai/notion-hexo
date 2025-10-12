---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YKYTE4HI%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T230045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICclSKL4N11JHLvnzHRMjmVh40MLAb8%2BBnXW8kF5Dcn%2FAiEAyZETVHhz6AJR48ZFj2rz%2BzMS3WeyxPkigxJq%2F9hwSeUq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDJqzuXT30WYX20gpdSrcA8wVL%2F9G8mnwKJusSEzuCm6OkdI8BSL%2BYl5FQOVo491uSfkndg8Mkr4%2FKPikCUbvfHrmYYyMD4FW0dBbX33hsLD12DOD9Wea3M0ac1Hx0KmA%2BtMiUAmPX9rq1tC0RlZ5B7bVSWUVyWaXQ6iSa54REmSJQ1rxqxmK1fAVBssb1TJ6BOvydcCymy2kQ94fhkrJcYkaM0zcAbcqM%2B7wn1FxutScArKUWauoLjVoKbmwAzpi8jaXrKVdJfrWTAp%2FgHcg1zXdKmFwkkw7jgmBQgHf31PAE9Z7CqZW0DSidsWdIAuO%2Bkyqw%2BXaaIcEonBbgFu1LyxhP7rYYeqCZ%2FX6XH4yYTwibfRC5CNc3LMP5BwEE7CMLht3Qe6uxihElHyUTfDII8lw1XpNiEvzNoGo5IKTrd0xSOCGxw%2Bg60LD1XzhawL3tsP5riZJvGL187bUa4YeTIM%2BdqL2NldUi7PKdfecXBsWrq%2Fn48sCBN4KUqEK39FmFrVfH0vTwQgyoMwhVBbFyu%2BPdqaeBc4enE8%2Fpd9%2BAfEblyLqOoCe3s%2Btf1VOL9nh8zuu5TmydTEzlIoE1oggRkJg2K5E5ZGAczxNjf97PO4ufAyycAjCpduRMg542pwyfctOUqZTbQ7UsIDzMNiJsMcGOqUBZzPfltrAJMvq%2FDe39SeytgEvHpr5hTGUryUeNq3TR1o%2FNswr%2Bt8r%2BDe3wdiU3gFfeRLGWDyhhtpq3WSlxD6BYzFUhA1bEvIwhgVoZM0GQ6GKxIrB36Ot7d0ktVHftVdojUT2QlCamdOpw7Lbzexg884npY4wZZZ2DNasPkTyj4kC1FbAZve1pF6rtJJD7NSQwJ2ewIehJm7q1Cye8yBvAZhFst9N&X-Amz-Signature=8bfba65745520bc10cfb46560290f20e1807b56d33b49ea38813e4c3c9f00ddc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

