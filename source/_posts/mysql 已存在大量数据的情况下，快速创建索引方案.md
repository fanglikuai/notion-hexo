---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UEIOB7CH%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T230045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJIMEYCIQCzWDEY3EyqTZlbL2c1jlfPApLweTsnkYJKy95LdDpAagIhAKyLt%2BqSaGM5CrdwIUQ33C4BiBaFHAb%2BCqMIR0zVVzX%2BKv8DCB8QABoMNjM3NDIzMTgzODA1Igyd5hhzLA4y89Igeysq3AM%2FybRCH4CEIshhNSJFcQnt17dMDAeEgxlwBUvnTG%2Fp4Fi7kIGDFuvdRkPRUA98d0SufOboHosHez7kqlxyB6eki97UrOm5Prc1P%2FlsAuoRc5385zDzsuBmW2W9lHqxYrHqxk7S6bKMWPG7X02Ln%2BW1A%2Flf5cNl4ksoYAGYdfg8vKI1mTZHJ0epRgErWJ%2F1tmGJ6lRNGdPOa0g%2BB%2BGmxjSyWp75JNBEenUlmD2azF2WTFVuWToy96r1FKi%2FWvxJ73CHr4vht2CC9F17Qwg%2BIE05LgLZEuVUppwU%2BX4YeLuP5%2FkQd%2BQp3amY7M94FjDfdpYJMra%2Bntx2RANOrt%2BzY95u0%2BMA4knXbEHGzQkiVEWncDJDRAyx%2FmzRnDxX6XdL713xM8S9qiSzrdUyeefbuLDuhSqlCWWtk%2BH9siwNxV1D91G5GopW2BoBaDDv4onNOQ776KOJu9O8coSN7mQb4Rx6thJZxMoiw4YxtHtoU6r0APYxnycJkRIN4ri2hwtSQ%2FSovXhrXIinQt9kDx0qq4wx%2FRWD3BeA%2FbNaKNeOtexsn5mTtDc4xucwrb1T69nLi9kRLsVxFFq1izE5h%2BMpoL8cFMY4Q6d%2Fy2Pgvmo%2Bbh1flV7qu%2Bx3UbyfF147jTDfpqvHBjqkAZrB7Nzt0WUjkamo5xjy1qY4d18v815yV%2BiYUZGjL6vGD1qvETmt0uCgV91ZbpTBQRV3w1al%2Fz20FHhUxpYBP9rqRCFjQrKTjUD7Hr1MkNOmky7WX%2BkFOD9Xff0DFOB7GPAHsOwSULgkc01YHaf216cR5arnROToV2joNXYGJLPo9UMym50rE4ea5PBVn92ZS%2BrvyyuT8vxeDZSWLpol64zji3N5&X-Amz-Signature=74fdb9761dc3beec1158dac1d389e8b7e7c6ab7944da94255ee528d04f627da7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

