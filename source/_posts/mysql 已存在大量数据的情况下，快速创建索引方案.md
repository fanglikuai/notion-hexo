---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X7MMYW76%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T070044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEcaCXVzLXdlc3QtMiJHMEUCIQDL6nc70aiP5TSbA9FeuFSn8PijkRX0VpA56DdwkKScnAIgNMO0KonGjMZZ8CZ8A937MCCoj2xDumEp9iWxH8pPC%2Boq%2FwMIEBAAGgw2Mzc0MjMxODM4MDUiDIoRyU90gtDY8sYwCircA1%2BaokROhwW72RT7%2BgzetV4whTJsFCmLc3mgt9iodrHbRf4jsUkf7QjPKTJBk0AXKIW5nZnR2hm228DZgvpRIcQlFQ6RmWwaY7bX%2F9fp%2Frf2N5Bp%2B%2BYWtBDNWawJ0RX31WzrcZxhAdjz9yBgq%2FNqDiOZi0Eh6f%2BaErdjMPUWziYTvfBDRO6HGOebHmtRstxOVZFMt4x2IQZ2heD5MQxTuS7fqL0TSSb%2FsJjBfnl5hkToyg4d3UUy%2F6urfUQ9W8aMF0wE7PxuZ9KMsmW3aLlciWsiPquS7%2FmrQzy%2B3WDjFBkukz%2FbAdVTC72JAUgIS0nwcJI8jj%2FmUTsFBek6GxsOQ%2BOziX0gMySZdR%2BNyaQ9cj%2FVc%2F8jbYVzH6SDwYE9pR0DbhFskgX%2FtDWv7Jzu0u8D2dOX74Tsu2ghHGoyTVKmmmfEMoG3A6YisYLRCyxBwGXhzHT7ZRlkcUnPXmFB5lLPmKahNwZfoM79Ye%2F3uZ5tRnBAheRsMw8hbhmIq4QAW09Q%2BgdmleddLjRplS3LaiakfQp8Y3xi2KJ%2FWAOcB%2B4rf7vMpmZgZAAFE2hgi6yfkUpOafkXGfnd1xDZN%2B85rDagKlmWoEMCKLmKV1fiF0Ok4me6dYNidBxTbiVkogTbMLWrkcgGOqUBm2K6Pjg0GAC8EvR4FZXEmw3imzb6Eg5lB2tREZzs%2FGqbKgiqI7aUkwS%2F6jKydP8R8qWSQgrqbxdqBcGWhH62o0aBqUM5a6aNVzFRfnzYcbCsK%2BjgV8clbrvYrXzCePZZ7a3NPbDHnIBKGvknGwg7XoXJT3CkqmE%2BDE4nFuISyvpLEE%2FrMK1xq1PvdTnOqFW5jyUAlPlAyCJpqRITrlU9NfQv64KL&X-Amz-Signature=62908e5f1da5c8d86c3a4e9ead06ed6a36880ad821dd439456d79a0211e9c466&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

