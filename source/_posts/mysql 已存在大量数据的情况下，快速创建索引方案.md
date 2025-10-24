---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UGAJZS7Y%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T040050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDBVvG9XqKG%2BDEa2WkWc4zmMPLY9XmrpjtWmXz%2FkSMLGAiAjhiImvdDWFaXiTT%2Bt%2FnydH5yGsGegT0ODAE1n6hyMtyr%2FAwhUEAAaDDYzNzQyMzE4MzgwNSIMgjriIbrWegbjGptSKtwDyG0TE1cXImFPFVgRNVwgwz2gvHVqObcGXIccjIv0T97msYdm18m2WTTC3hiIWCeJZHAv%2FsFoaS3KWwng%2BRDSd5DR82%2FfiLZiCZ8D%2B%2BrxDSFUuHlv9QW8OmwHvN%2BuEOSMDpeJJxIxnQf3HZej%2FilUhH%2F76jxy22XL4cX28tI91CNM5NeJJJjaCqlcRLaPkPLq7dmwYDBGOOqigaLsxyu%2BbtbOox9u4vE9Z0SdLEpkorCNGC3M2BxXtDK%2FWvix1cIeIF5vhfMdiJHdWqBosD9uJSC3%2B58G3YraLJxYjbXSj8Laq1HTnfkdCtwpq6JElrnS5864KCkEFWre9o%2FcxfWTNKYMFZz6EXpeYySciFqhaEeDmLP%2FsiNyq74KbJfS5RQq09VRyB8TgyxV9tm7Jd9TpHjlZeyMjE1XsTU2TyrrAhljNVDMmyszeLWL1NQo52E6UkFECsZabA84Bk3we6CQaJHeacNZgXVD2Clx0F3NOhArJ%2B7Ae0635MXyuDdqfhdXSajiaqDZHeJL0Zbdv3DAotyiHeA5dzyGJJfn%2BL5xnCbaZEageiiL7OVVjyOQzdpXESzuL72Fm0KgyxxkGeeIFUgg8I7MlRyPjLIpgI%2BBUSr3PIUF2MJRFf7qEzMwm8vrxwY6pgFvzHPMdKsG606bsgd09efuEuIIXHVZ5XoVaJwADZnN2aykC0bq7EEpKRQNhagdx3fJRP1bSd5VL3IaClSvW55yuAIEc2RxcDiYWZFvb2H5nuAm0fRcDSh%2FJHrqT4oMWNCmV0FEsPzh5pGKYEQmsk0bWz8g45GGFX1zTg2kzlPjN3gUNmqQpPSNhEQd5X0x0LqS2UXlwBsEUghnnlBHlNCYVI2hSELY&X-Amz-Signature=f0756280207b168426e6a565fd412ef583693a190db82848ee48bc44b3ce21fd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

