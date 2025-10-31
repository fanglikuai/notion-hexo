---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VUWNDMXZ%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T050053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJGMEQCIGcalp7HaW9boQCXr5XYv7fAccieDtgu0rxBo7%2Bz2gyTAiBI7bifevhq68Lm4ma%2FRAWWHEh1dZmFiYfzhfIW2%2BqlkyqIBAj9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2BvtGXYObtJY%2Bk8oHKtwDSzpkwYoSpWrhRo0RgTPlEZnlCHOvP%2F4f91I2x3IocDZmw6wXkRcjkHRiIVaK3oJGqXeel1FJTrvVyfY7HnIvuq6kMLUPOTp9MlB%2FjC20p1%2FQGFezebIi7tRsNxhQAMPZp9m5ARDR2yRcOtMYTXmyJdG0zyYQvCpobMN9GTb5szvK6A9WVSSpYAkSSDNax9a%2BTKQMmGBIxN1qBPjq5TamDVTSbNoWAnn7VDYEhgHzZItb5ST0Kz9owscnB9bx3EToeL%2BRNzypAVmUh9teiOiM7coC4yA2rW3OWl82NjRsM4%2FnSoZ4UdyJHkbKAPPmyKCbrpB8r%2FnVF4NGKmPmO3eTp0BHGaG7sKxSRPgOSdu59v%2BK%2Fxx4xeMV1ou0yBc%2BIym392u6vFD9fjidnFGF27V5uMbGxRF5MzX%2Bg%2BGo7R7yDIy%2FnGcu%2BmLJn9BqQHtFdqn%2FdpkqX0DBp7bkSNLgsluxr1LnH%2BEQNnjRKv%2BHO39q24ZFYzg%2FZ2ZUTWDe3qacr2TPy14YdmzZhrw2aqiBp9uVF9wvRzYYePXjWqVRv%2FgnADm8CCL%2FgN9%2FD%2FxJZ3V8DVuCSg%2BKUJ2%2FIQyBTdhDI4EQyKJj5ANbSRT2bPkBvuPScmkMwsDlNcSZ3QskUFMwuu2QyAY6pgHpo7OqRy0uyDWVd5NmLoeEG7ADoQcU2xoiGEYroeh1GCdWfJNS4yOQZRnSyTPfTtWX0q%2FKFJ%2FpWXKN3Dd92cFM6VGMyVNfOkEQAHh6EN4Xv49qhSRONap%2BP4%2BFcr4mIkrFC%2BhJvTQumgyfF6bB5hTKCRu%2F%2F%2BjdafsjaAgp%2Bol33hKW4X%2FIba%2BfpIZCsUnoFWR5UKifbF02vl6MxV%2BiMxfuF8b%2FXYGh&X-Amz-Signature=ba16afc67465e0233d4d59324311eaadfbed1469e0f512765f245342f9328961&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

