---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ADBN5RS%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T100041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJHMEUCIQDtU%2BDg70k9h1vveDG5UJTDdLHM4e%2BocudJTrk3vNF8%2BwIgMSVk9v4%2FHFCtBkK2Lgww3%2BOIxaDTJGIdat%2BUnZmJbEUqiAQI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLuDUU8fQizuuc5XcyrcA58PlmUgBPvhhxNAu%2BfH%2BHmN3LYRowbHXkEDRmQy%2B%2FZDPPUJ3RMweg1QkyfxjtsQsbMKGnEV8prPL8b%2F%2FmaB4l%2FWk0dCiwoZxMxnLnBH%2F%2FMH3QU8Up1%2F7X2nXr5WZJaeC1gcgfAqLnH4f3AVYVlcFWiZCzZGgAN6OcfAbsgQ8IvfvR7QVMwTa%2BC6BTB5T9bYGhQX5RAJnsQSKP%2BA89E2ISzuKY52UNPFJzRuvpV8ntP5rTQzGT0%2B54DrkzadVo6z5RF89HtGaMh7V7VwgxEaebaQoTSRYzlfPtZ1NU9qdfwCrnMxty41Ge44oh%2FVofmhKhmv71LHV79bn%2BmHR6cGz4iE9DL2Xgi9RVtRTrQB2TZBVxz5H2xynpOMYZAgaZB6T31ieyfVTMzao6VWe3jKgV9c1SwHRcCDBZ7bwxly9etmGtESZQrf3go4uhACAyEy3O9zql6mZkI%2BH4ElcC6lQZN2vrXvb2PCwrKCnuCs9M%2BShYB0aMRTaTwvwZjmTCNbkQcrfLh%2BHFUtN6I%2BY4U0iFjAY%2BCEXojdCWAkpfNBKKibmprsRC55C2hnySaw84ocIl8D%2Fl6M3GK3m0ZhTi8YZayHYBCGNfEoarKeRirohnoiKib4xQsYlANhGO79MPrip8cGOqUBexf091CX1ocIk5erp%2BBAo%2Bg6dK5XTF%2BxGzbinZA%2Fxzof%2BxfKz%2BtU6g8libOXS%2FJymrP%2F6cKkWRv2kL5a63Q7Id62ntnWltYsfnY55lU4%2FF9UcJ1KHtC3L0fwnGUx8eTchT9chCdrOOA%2BAyKsSKJOcNd4Zcfd2w43iv5HSRtjwOsoUMD1y2hy06PDNy7q8oyASpA8bMH493SDErJ0vYkZ8QqJ0KlE&X-Amz-Signature=3639a46c2159123cf92912ee5e16112cf427174b2b1804ef9715dceaddd47a36&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

