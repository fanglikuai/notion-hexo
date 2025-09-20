---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WEK7UCFM%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJHMEUCIQD8JaTz%2Fj1soRzifOoIHTWQkB6Et6B4zW%2By8iEXOkRxKAIgRVs0PI1FuEcgE88iaJ13HP%2Fv7gG9sAeTrY1lvN%2BeX7gqiAQI9P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDP26T1eDy%2FLsbNl3ICrcA8bSFuZC6MYe8f9Nwvj792Y%2BhPS%2FoO3O5g%2BP%2BS2%2FuZfubXPjWDXgGn%2F79kdzSNHHjGjP7HEHbgo4o%2BI0WTCxZdsmVCe7g0XFbYHYWeosRMGpyVoLvfm%2BGeP%2BP4cNrOGXoOoGqr%2FrNgfjzW6BmL9hx0edN1ubrhFSFmq1E%2F0lt3ReEupVFEllbleVyWKhi7tBny%2BCJSIfUG%2F6U0vnl9a94qyY8aT5z%2FZHeuNoDM1vH8Y2HPwFNH29FPkBeLiTsAdX6hTvEgc%2FU3RupOnWJOXLqpYOaN8xHnvZ34X8a8h5c8ZNojJVbPTf57L4DXfNDlU5mZTBBW5gUUPDuqAQr6RbqoI4X1Bq9mVF58Ek9qLgXfuOWv4AhQbEy7n6g7HCKNk78%2B%2BL5i%2B9QPr2zylz666XtRdTDFWhH6Bi%2F6CHcqP7L3afVLF7AyzCFJ2vuuEZIwBH%2BSJguVNuFLC3EwMrmd6BSRcExoGED%2BwzJpmrR%2FrqphGS8NukVSV%2BD1uQp3jOCK3KCSXfhPMB77KEDkUM5vWs8PQrb9fpnrUef%2BQAO%2FRj1QgxW3OA3JlLw%2Bw3voZp01EE%2FScqgcMFoPO33DsKgDLEC2LoujZ1Yfz%2F7QG160KuPSA%2B35s8IsU7L1jsRZsqMOn2u8YGOqUBpShpbIwnWJkqHrJFKNT%2FOrcN4S8l14FH3ztPFRpWDz9K%2BGsSDh0odpiOrrUhbTLjeijklLyNJy9pW8kCmZeMiTqkn6OgeWqU9YvesrZFaJP%2BTNNK%2FWykn42TnFEL5psfZ2WeQ0%2FaHQ79xSRwYr5%2Ftf8HFHdq%2BmOCzuTnmTD8vh8wPVxtPjNhb4FUSxvjKy2jmVZJLggUofEl0ZDlbVEtgJU7c3qC&X-Amz-Signature=13a19ec6dfa57167269b568dfde281342427a9f139b6705c0f8b7bb5612a23cf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

