---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466THKWRGYA%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T120049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJGMEQCIHkhI6CSXOxc0e2coUNbwOvLIxNDTYvjyoY6CknLXqURAiADYlH9IZTDw3eHM%2FkPbVI4B0AFGmqKU6AFc6ECabN7KSqIBAjd%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMp%2FAwN5k5p53qeZgEKtwDsFr168epwgipKpQivQknWAsTdt0kJ99rI7DamLYiwInmwH5%2F3x1vsBqena4Nl0gb6km%2FX2jHXCQphLe8YH4bniFfO4sQbiHEW7dpuWFmDdFQZsHydDx%2BdfayNsT0w5QxRsMHL55OpgbVx385JotSBF548Y3Ciglz0qEayUNnP8ImBmAGXrKobdQbwst%2FnFMGR0yqxFI0ojBYIStiWYiNHCf9HEq5Pev07jeUHWez9nqqcbnToPAkupYK93VWwhfIaraKvXQ5xVUuMcTddP9fYBXRj87MFVHcogEWRm9pl4pkaPwAqVaIn8YFymTvs3r%2FF9AFtOC2IqcId%2F5xjsWHknCBW1YQ%2B5XpP1FNGNEO%2BWdthbTf6AJvyfDQdSS1Hp0EakaW1jklOn0I76C%2BgaewgYF1AH5rfNCHLunWdDRzuFpb7N2gMo4FqmWSf0ngcCgGLS0tvRhqCRGtatqvYIt26UQIxtEGbF38nYCL9LVZHPp4%2FvZ3rkA4N3SX8grnL%2FrrMAK7Ouoh2AacOJJtClX9U03pmUU2nfz8C0qctA54zPaQr9%2BL99ohSUr7Bm2Jewqyll1MCM3eU8b5IpSXncl9lrJFcBKkjquXtuMt1R%2Bai6LynruuS9s0lur8U%2F0widP2yAY6pgF1vSCvbcS1v1bLYDo0cDAVQXi%2Ff0p%2BDl2JaKoOWU2IEGxQ76vCCVI9IOWOPP28EFDQmnI34T2dkLbq%2FMP0XT3JRKSYrKzSpqjAEJKNHhV3DTr31xBqtTDtEsKTXDbN80XZ2i0tvSBsuAej2OEUCmgTetO1zqHjvCI7JVSM%2FgcpMUZ%2FMxxVEf316e3jOm1MmpuMFqD9HZL5lhD56tbuWncOliAPAaFW&X-Amz-Signature=a375df7e853129009202f2f5b997303938ac1a881f5cc4b65a41ca02d12dc064&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

