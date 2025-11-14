---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T24IOI3G%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDqWRH9U1%2FUGObmpeu1lA%2FusuQcWVZcm2PymXYjCUTvGgIhANCt4qXaFQRF9HOPb9ELwZQyuBPu6EKrQBDIZtvOdjGeKv8DCG4QABoMNjM3NDIzMTgzODA1Igy3BBPzNtxLY53C7MEq3ANQpVbEzRI2JKeT6803RewN9LxTn8eKZGO9iwy3xBUrqpVqlaLR0K%2FGh6EtuNStUjgAidLZ%2BdGPzfUGF2ClDth%2Ff9mMOXBcQH3XeTqPjzY3zIqBGH%2B2aYstUVrCd2DQTzqBmUKO7U8WYzcJKwhIb9YvxwI2mI4NW9HOplskzbGBqGrKB%2BhZMp1J8RQP%2BPSmql1XCMPEjgTIvAFM9MOXwkGhpNVDu7orWXBEkBLuQ6GnjVPkTUO8KUbAJZljBLUzq8090j%2FEORxPb5hS6VJtDbubnhpy0IsEQ9KcvmHRYP%2BH%2F9HuFbOD2AQ2Em5Kn1xlRmVrQn1K8%2Bc%2FxdFTaF1b%2B081Av0cytEmAW2%2BTAQZ9PWIA9yjTYfwI6zBEFMHUejdwpHugcK0eCwjvJzhGmRAvTXYqKVvD3bwa5Jy4c7Af37cRjGi5tsLpsTDYOXbTiYXpcDcEGY9iTPS1DYArPjPVd8V%2BmHMqSZ4o%2BdVSYEOBbyXwXFUiF3v5fFbXOn1Y7hknXrHg1U%2BLIIQUuuPzm4c0e4ZR0VjDo4ds8sNbjMZeCbAPRVvPE7n5vnoiWT8DP6AUOEuXX6xfotybe00ZMlxO7SAi8DYTx645LZwyRPECTcQWvSPN2BaZdUODVEhxzDOtd7IBjqkAWxPVzQJeYgavJ1qPytbAwXFCw8k3FYi0uSN8ij0%2FesiWy4LFhuQW4CoDUCOPMJSyQM2tyVGKFr0IAy9EJIHYGRihSjoAuH18XUau5t1Ha2V130BibpMcgvsB1XSqfRcflqtHc1tY7b7HQBsCLthknCRePt9W8GT9JBhLdyyXwiaETuYSlAOMJ8L7hbo6pk5pMb37H68xKqNCf2LpjVcoP41XXuY&X-Amz-Signature=a9496492751b27545342e89d51b821d43d88f18dbeb6127f8d9bff75b101e02e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

