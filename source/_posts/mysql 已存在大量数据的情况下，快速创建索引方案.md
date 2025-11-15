---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RTNZZ6AZ%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCEcKgiwzHVGtzAnaSBm17WZbzrtqfbox6wlaJMVIeohgIhAO8xxsDGtle26kfjnOqQdIErnOirQjsE2hsYTbqurUyiKogECIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwp563PYWNwmfzry1Eq3AOi%2FeF4XUS6O%2BYeXxgxRp5L62GFQO9gWTQS76mR0cUExuQQJAkFH9HZBe5zNhbuSPwWPjOo0FaDnVOD73CfE3F4fNsbzXpfNjLmCJrLlSY0d5AgOtSYd1OJRydc9PGow4W9%2BHB8uehtn1qvf1izzZsVIXe6%2BWtpSdntj4%2FE7wt%2BeGDIhGpmFa0wvBJo%2FjQj1hvVz0hsPkHGVMMrld8GeicBZjQOPmajc5EkWbW06DpL0p%2FHq8bCnbirnEqTBirBZrAkYtLakqVzKTiaF8l7njgei0aM9tbK6KTRm8v2rpff8%2Beq3oorRqZemrHrBNblJ1Mz05OjTnAw0qozIqD1aB3pw5mYltv76MomMXlBNlR7DvGlJNhXIAGxBgFk1B98gLXWq2uOXviDuOSmcCSJd5%2FqOS8GdL1l9lSvNETCWHDCcghdkqLTL%2F7JB0LP5ZO0Gt746RXP1FkF%2BqtZhZd%2Bh8k62EbhsE3di%2FwgQ7MbB2Boyc8riMsq8fXF3bnWq%2BierjLq1yukJ955F0S127QnYJ%2FTet5CXffJ8UFWaLkECqFOqCFn196t9L%2Bwh%2BKtbyZ9WE7noPW5FbYaddtpVNvHfJCFqawe9GmAe65gs3%2BM6zSf4J5MXAdOqOU0HJ5WojD9xOPIBjqkAZ4YQBiBVLvhvmhOOUZI2QAZGF0NK53QuVS%2Bw53rHYVd%2BhdgITw1KHNev69OUGcvtbS6o%2BwWydeiHhOyX2TvF3kkzXsMSFobl0x1yvBBPZe17cpZXEljrfUnuc4NUorHnzx8x30Lm2nHA4T0hTiZNCgn0h36fytQXzGBNab%2BCrxwE4rhqMZEi9ZR%2BcCC7V1I%2Fq6chQEMUZ%2B7kbPkiG%2FKCSK9bOwU&X-Amz-Signature=f13bc423f909869d3a063817860504acd4d021b4974150c07181ef52b7a2c5ab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

