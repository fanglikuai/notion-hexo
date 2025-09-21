---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RRXUVK57%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICe4qlfILdN9px4msIaUiJCdW8Kte1yZ7EG3p9sy2k8jAiBzjYIDAefD%2BLZ0Te%2BCqaXniIuDOmbHiRiYLfAR0dk%2F7CqIBAj%2B%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMQ6sNvRqTURCPofeAKtwDks%2BbkVQ29pYn9QRwg0bgsnNSmyL4tDPqPbP8dDjAHu0Y8Oi4L1akaDz%2BRYN%2B8uvPQ3lV7e7O8nN02KpRSrNMSuo89%2BUevmTxZJj6llvnKZhckZF26hNnKLgLGm8%2BfsK5blEWHX5BRssi%2BQNZM5nxslv2J3D%2BepvxEBd8hnIdGoqB9NDo%2BxvBz0YRjWHncaeUq0oWcrrK3VJfHm8uUgbIRAtReOT9QAkaLt4qjM7pV4oDbFX2VIGyjic8DLjoZtiwZBTZysdWrGa2QmG47SQgUsLh19sfxz1hW39hIARW%2BjQK91IKMIO4%2F%2FqhkcmxXkSwS4yjO5TCTqQyo40WhHKyDPDGPBDzrb59KLttpbiKUyBqaKm9i19V7qUtNHTHtjiiQkSOoanFw2VOCB46w4Y6wA5b%2BUN8p%2BJUDh8p23JjetrNgZY2dE%2FEjsEszVZzSdwAxtdNc5cEhYunsJWPalv6tg9UC4NG6gG%2FP6y1twff213xhJho03gbdUMSgX%2BTSkGCF3A2j7pMl1UfyZ28ZLXyQT9rZsardZ8fUVCBTWu2vphjCmDHamMVlHzyl19izLsyZwgYcYP%2Fkv12ESnGKnDwEDYTXbCIZsG3KvbtsxZ5Qs24FObFxyrh6l5Ab5wwif%2B9xgY6pgFnqd%2BwGz5Ip%2BWEXUJbmJI0oFzFEKGur8wq8uZw6bbg3GYGak6Tuuez6KJRfcbGv64ZqVzR2c9ugBM8mlGdF1zkc%2BrDV%2Fp9z%2BqiVDRcc0Gd8NZljcuBXaZpfzypeie7aCqFUd4Ohn1a8UJ606%2BOpOd811Mcp1mFCjD6UjpuZ0FCDbsqJo3SprqqnQbKbsINf5EnXm8tCs9ZWoGSvACcy6WQ3YKRvP%2B2&X-Amz-Signature=3615d03a4207b179c74bb08de78b27c408f5fb8df6432a472ad2b421187893f2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

