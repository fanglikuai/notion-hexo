---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TMKHPWAU%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T190048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJHMEUCIHb4%2FAetjurL2CTUgK%2BUnRBIKzSELMkpJYgbD58vV%2FuWAiEAlroMO6hBA8v99zzXBwKhCgaVqJMcKvluFK%2BPKPeK5DQq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDH7fmrK3hDeEM4rrUyrcA%2BfS3RFKwn0X8oxDjQ%2BNi%2BuAsYJFhI0AHHwvmpeyTGzOJG0lafy45u2Xj5jPQRg0Qds6R589Qc61RvskYXa7xy122jFta4bDQGTgkivFWfV2tAGE9WfGbpPvgUJF9EA6457IHQJUaQgONuhUFvBOwnwHwQBZGq3eakqnc7EK%2Fm4oYKAPDOuBMHtCKYvZTxYNgfpLszIkIiVFpRCadN0pD%2F9%2FQBVFfW2bQcLYvwWckYZUmS%2Be1a62PqMOE2gv5D13tIjAb6D3hhv7npiPoHNRs4uTSGgmhtYunylJ54NZxr7DBOjXFy8tlWyOzJ5a%2FTI3RVLIWkoT5v42LazcZbseIjprHfesB0WuE4%2BzS03NATxpzLiKQy%2F6T8GD%2Bbhbz3SWtHs5d1DG750n8%2B0V9p3e2EZmbPmDLxIG0GkX0qIZk9gc2zQ3HkPKWMLlvb5H3lZkbeYqXmQAqyD2MvMzkthbdIR8gDn0s%2Bj0B5E5Tt5JcpCVNZ4hHU0w%2F%2B3RyXTDTj%2F7UcBEQnQ%2FHevD9zhi%2FaNBS7zKUUzg0TE9S6Q%2FleTjmdLz01BR8gJrRh0RQi%2BJRSWbZ0R5kfwQVzo6KVqgrN2ZAfETo3lSAM%2BWvqLH2fWrLIw4pt6zrze18M8Rkq6jMOX%2Fk8gGOqUBuP0L9oJAlWBHBQh2UORewMstMBoW5ut4qYH97RJvrMBVjnijBdYrwh1HAQkMBMXOIe6YE0wKg8tm8zbc9ZvQjePyFpKIsm6D1M0Vhfd24rKlf2Gf09YccUSMIC5%2BUY6mAB5HROCtz1CTUmaYzE0xBY%2BvX6%2BJT%2Fc5mj3AGpWkeNpnBxwHgYNBVhpwlBTV%2FW3wyzLb0HR6gfVpHv3IrWFlIo%2FCak8I&X-Amz-Signature=4d6f2d008462fdcb385afcbc74bf57fb4c040efe3b7b2016a0d03636645fe1ee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

