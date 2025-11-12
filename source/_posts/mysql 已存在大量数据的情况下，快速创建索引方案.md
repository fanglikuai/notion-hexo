---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDHHP6XR%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T200048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHQaCXVzLXdlc3QtMiJIMEYCIQC6r7evkeIUzedqR1Nnoei0XblccfcD19aX6CaBV8cPqAIhAKBz7SmbFEmlj%2BBwlvEwdR6cmWrneC9a8afN3xYidtnRKv8DCD0QABoMNjM3NDIzMTgzODA1Igxj59NF8jyGYwReNDIq3AOrwMJpUAvstZBv6WyXT6glV5o5UozVomO2BLKrlVcMnzZKaZNA%2Fp8PVG15PJoeUY79ryHGPN4M8jabZIAiLw%2FNe0X1a4h7RMsDUpUnC4%2F68OXrWvwi2ue19u52zqiWm7Hi16F4s4f2HeaHWKZjAFRdDQG%2FShxEi%2BT8tkvn2jpFmkyJBIDGLOjuWPwBgMKuXAXJFmr5dSg3LlKFz%2BWvH5%2Bxzc3dwVZcgvRoA%2Fro60pICvjf3v1mRrxfw33UGmi7wAJrNTxc9GP9U0DsSdBlGBRhdd6NiZXZMw90rogkdxqI7UdOB5YBOVwqGpMWv1Ii%2B30w%2F0fdQUyWMW5gOMTX2zK2HsRjYTsI2gShERbps9WsTyPjS0z11vG%2FlHPE0qtAfoemUyYF8KNJ1dxOOFSgY%2BL80HzRIRic2JWx9qTxelprtSwO%2Bx340W2rMKbB4vP1nFWBaGHzvbbms0WUWNVut7%2BEfQ%2B%2Fvo7T%2F5qola2L%2Fp827PgC7Fzt2Qn77SJM%2FVSnglpWPH0J6jdkxtQS4pzJU40Tr0wyq4Ya8yPUdYDHRyIAD2aZ1jqTFbVasNcfBtKy4%2B%2BtbAADq73BGlsr%2Fn5RUsWtnAYhm5ebWCzAL7VEvWVHicZ5WH9vhhtjs0po1TDnwdPIBjqkAaFIbkrlF%2F6kdny%2BGVbXopGZkBoOG8UD8%2BD05sE5AygLHCblxCQ6oeZleXBM%2BsA2UavsI2oTVOOB4%2BhO%2FpaBYBj5THMzG%2F3IvYQW9EErymwm69Iz%2B7%2FyPugJTVsaAKG0n9qFRLrHoj7F0jy3S4btq6rWdj1SMH03n3jMXZN4aY2MPDmpd0ZqhI%2FrFq3rRK%2BZung72B3MUtZfRt5zPzxjkALWSXjz&X-Amz-Signature=ba5ddc7306fa7106645dc4bfcd1e7a03923defdb081260e03abf70f80bb706ae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

