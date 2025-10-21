---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QR22CFBI%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T150140Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJIMEYCIQDvcs2APVE47m%2FJpzivmLFwT264MKVOndWEw3R4BwpacQIhAM4BsB3J571dUp2pMmci8Rhyy9fJhaHFxcwK45aa46bwKv8DCBcQABoMNjM3NDIzMTgzODA1IgzStj0ZgeoGqbuQeC4q3AO3gJNLFmjqtkduI%2Fi7SQEnYa%2Faeid91m8AG2KVsKjCdghXif7y%2BhstAmdepNPM9FIqQaZ4DmCfCeoG1abvWBuAznxWr1jsmXhVyKuXs4bz8GZCta5cCiQL1dsab0Iy4Y6Uft%2BrVQwWpqW3Adpb7jVucIng5kye0IF%2Fj33%2FwNz8xvTIEgERb59W8L9fRjLqvOA%2BNFtFcdL7rXYZwyWoe6b8ha89qX0XkSIXDlwxw4E9SH9tDGddK%2BNttLQwHKcxPeZ68R6%2BRWbBTmvu49X3ViN469E7%2Bnd9R9NCxpdU5LYlY6Qm4Eqd9Qc42OCGeuvKrt5pKDGTCaQ9DhZIYLdK0YkKSyl%2F5DjmIrO9aDohMBVA0l3WNJTYpQLzOY4PT%2BkjhoUOCJ7jzsq6WAOFIiBgKAqD3MznIYtmBBdXfXMpr3h%2BjikKtxLXPCqMQPH0DFWnYJDsCd%2BeCxjPXwGbst1Lcjrjf3psMnwkZokqaQ9xr%2BNALSClsqj0jUbeABOJvdQt9Gdoz4HRmRE%2BPMCmrh3y%2FU2rmAErG6deXUMKSW4OUNrwbh9Xyir7UAQKCdzx5mGMGdJvB6DtE6sfSk14IC5uf5efYzm%2B7i%2F2Uz6MpefBr0wOMEsBDYOy%2FHShMZkzhTCprt7HBjqkASL8KRW1l9%2FdLPj8C8MgEokV2JVGwJ%2FHDPsoXqK3PMJAfQUhgwfbKz1tnQEoRBh1HI9hY0HhzFLLj90ZiWHNswvAGx0z4Pm8Ez3rC5UZrtKvZ1GB6HUHIN5EFS3Bdd13q%2Bqp9JNNRXcbk2dPuF7roP3E3XjETLeT1GaIl8%2FPKiUhYbjCGnZ5EHI%2BR%2Fp%2Bq0AonGJNUKb6UyFmmp6jKLC%2BDJBRJG2P&X-Amz-Signature=122874a30b2fda050266a02166a0ca0b8e0911aa4c622d398aafaa677042b7cb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

