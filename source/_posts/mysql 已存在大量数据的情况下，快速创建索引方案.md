---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665LCDEETV%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T170057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBv6w6n%2BR%2B8RImPW6K6KCtf9Mg%2BnzNOTt%2Fvw6r2gJe4%2FAiAPX32FADjYBx2Q76QGfpe43zlpvPqMAMC3gr%2FC3akqjyr%2FAwhJEAAaDDYzNzQyMzE4MzgwNSIM7kHYbxCqpDxXThU2KtwDHv6ueFlc8m57AdAtA%2FNoWj%2FWOpy%2FRLPTkr%2BwIZEonqHXOhAlWr4Yq4%2FeN5LcKermvAWYhMibV4%2BtzY0nV5%2FlSuM5OyfUxmD29mIPO5p2Jri6wSRK9HyN7kpzyQZConkkk8BhqFL6DtLkFA3%2BRvOEBx%2Bng8yMhtDLBN%2F0VM2fiJJUiIKJ7ewwWtm4agWd81RDjjTCw3XyYNPN0G4q2xXzK%2B87KECd7dauX9O5Mf2FgPbk0eYGzXN61EmSZLmDCrZDHNzJrOH%2F0yqOblpYzGwRjEpzDkUlRXt3u5W2h6NW%2BUn00jjLmsxOXpKlC%2BR0hGzVlH7FswJGdlQePLM6MUtwtvMXh6pi0U%2FetIMVo0MhtrJaUb9SoEs2mUTir39a7VogcxTm%2FoigNmfoebH1dQ4ddCEzwY7QNoeOSjlz1v3TTmO0Gc7CTnCbCls87wQtVDmOhF6GqJ27ifj40w0JeevW0eUdGYGzIlhRWQcf%2Ff8k5HcZEjpCjd5TgewlJsBGUM6jGzIn9PZ8FZiwD51qtrGEx1Al5oedL%2BN5z6xiM4jbgD3Ap9VdiEwIlnGv9AJFUYHq8bbNrtKKrAoOFOagaUB69iaeuOltzgsYiSkltAtQlCXS%2Bj0w7hLCGn4A%2Bt0w4pzpxwY6pgHqT95uxPdlKvQV%2BX8xYea7g%2BLIkwViSTyyNeLuyrYx3LBbg9iut1VTGnM6p4HIsupuVq1pvGQmQUJoSbG9a%2F%2FVgi6IKrcaT2SsmT941BSt8FdqgZpS6K1F0w24qFhr7xJQ12yXm%2FV1MxafBZhARIunFdtBV5%2BOP036Sbts5Y5xmCrggBKFvHtLPPZscVcy4O98gZTJXYVjil%2B36iAveYZieNmha%2FFH&X-Amz-Signature=7451b72a11256ed0c6f46382d9aaf7782313a10c8c5a84b22a5853645363a35b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

