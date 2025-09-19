---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQOLKRDU%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T140043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJHMEUCIQDNZr4Ft9NheQtiNYAUTGRl%2BFcZ4AuG9h%2BWmyHTnjAbigIgfF5MmtdkubUsggHSi9J5XZZ5hk9idipuWC8jLbu6ViIqiAQI1P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJT9odh0Z4t41NR4uSrcA9ja%2BnnNuKvZRGejlOgr6J1GTjHnE6%2BJyw%2F4xYWZQbB%2FQYDADVZ9wbS6sUwGULKjYWVFipz9gmcKKnQVUM1iHYiWqqLi1rUZd02FwYuK%2FSJc5A8ZJy7EUwdzf%2FNsWAx80c%2FtbIwq2zokifrsRM3rHrLHaMF2SVfozR8KgMn8ykxUoX%2FC%2By0s9gNEuiXzW0we3Dp78uFBjOiILDfeMDelklVwI5L4EoZkHsSsxztcze2mcs4oBHx4AcDjfOxSqgJxS1SEN%2B8il0ayPkKxo69mbdTNbQ8mGdcp7%2FkA0rg%2FwOS5R00cKRQcXj2IBNi9MiYDjfGxgJ%2B3pBVMbA09QlvQpKSq0rG10VXRFt81sQvaSUrLOHvIQzHCB9%2FbFIdrW%2FQQifv3m%2FtHqY38fyuJOiSs6jN13zppR7yJ7kKeMhvv1Pxzje87oGmyg2MH%2B%2BOfuWmxZiaf57NnAzJGDsRiRbZcgOd8z6tQiyDPJ4b%2BS%2BQYfT3wnJnAr9%2FB%2FwwHUuNGFc9i9vXrNyJ4TL%2FQ1q1li64Lolox7bBtbe9Vu7IYKyLXXh4zVzpxrm4C6vmndzM5oQhmpfHHu5lKaB3FZj97OISW4Sf%2Bv3d%2FbDg1RlTn6tpRnP6xDP1i%2FrPOsOTDZsxOMJzjtMYGOqUBMshz0itXesrwYhB5t6UcCWzGVavRLNdV57NWtpAmzMCJ0UPDcr9SGUCGcQpD%2FuohuLmf55%2Fr%2FIlF6tBhFpM%2BwTDE%2Ft47r%2FYM1RLoMOLujcJTgBjkE7SpgYtNCsAFepQ2w2EO74llqlTKGGKV%2BxnAw%2Bx07wfy5gllYqJrVgP3dbv2I2O%2BD1fP%2FSNTUBDPGkrkY%2FKmVqjRzJj3GJwwjVePlE%2FJRBwY&X-Amz-Signature=4c4e3d000929fcf3dc02d93764df7a57fd691ae0387462a69048e5f29793ba39&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

