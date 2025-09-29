---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJ6OOA64%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T130045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJIMEYCIQDd1sGhjf%2FUlujCUEDjOyyqZfFOmHaguxGTKeAyKhawOwIhAMb%2FydYA%2Fs%2BJSaTfo4hArtea5hWnTiOHFWyCpZvLxPpPKogECNT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxA5DL7ytMg8UL%2FQbIq3ANmSpeRAVQ%2BrsJj3FYcWCiGdY3ce73dbk2itohfLLY7bUE9du%2F%2B6lgdq%2Bls7qCUYCCvaGsQikSOTB8Y11Gs7%2B67ZcqIds6o2ODDEAcuBXr6%2FJYnDtBqh0al6Rbfhy4JH%2BpEMkbYRDXOMDOACR1AgPoAYWfsZyxzdDLJd%2BWwGNGVNEq%2BpYPG5QV6J99b2m%2F1U3GBBjHogWCKjYnawRWY17dlRQM1QpZTAIiiCuuD6kCDj%2BzZO%2Fkjrw7nAnA71avC5jADitzy073aBA49WTRmJjh5pH0Odw44lixISAD3HfVTYEXDY%2F2CwwQno8DsnmVw7y%2FAl7LGqJ55x6QmM5q%2BQfpRX0VGWO%2B08ZHxs79UlU4T1JxfW2GWQk1Opa8G0BY0dRUP9BZcrd38pfRxpQi8wdy7zbScExhxEZdA%2FasVtEl%2F2QLamc3L1CVH%2B3sZvzaXhi3magH6BwVFgolQk9Wr9DUzyXRdgMt4LzAwD3t4VPORhpOgaEDvPHq9IWXCddNaqLjgBlg3Jp3E4gPVD4bu03n8u6DrIg80B8II5j2ttm12%2BCW5JWftIyMQNmBlxm2%2BNgry8lEOgwz1Lii0xgHMmF4exy9m%2F7cRsxoPMVD1w0daBLd53LhtgOlIq1OGdzCz1unGBjqkAQbab4%2Fuw1lZXfcc7BeciT63zX69F%2BtlToF6H2OyiXewtLBB%2FgDfqP0sbskmTC7of%2FxWiniberpdMuAPWyQxYFZ2SBXXF%2FjRu3cUMcdeuWdH%2FXV5EWyrS%2Fyu1aGA%2FUbjURW%2BxzIkkCmRyl1DFfhFh62fPLbT3ppkS%2BWylcH3bq96brl5OrIGSNS7UDXGvR4wHLc0Eknpbs2f944JUMdSczet%2FOD5&X-Amz-Signature=68c2289ecfeb4409406d31888951329c699940b4fd2b12eb6ff3d6d45484f43e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

