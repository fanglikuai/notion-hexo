---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662B7JXDTF%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T100054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC3%2FgEE%2Bvbqrm9b47DblIxD6n0Wd3BqTtqAgnxgJ%2BSnqgIgQ6LCxdR3Vi5eZyCMTzn7qnFbT9v4yrLGvFJ8O9Y%2FpQ4q%2FwMIQxAAGgw2Mzc0MjMxODM4MDUiDJQAYtk51HuRQisDGSrcA5Z0jHORu1%2BZO4s3EMdObFEapr0UwACyvfhgC2wiiVFnv5K2ciUisodf%2FkABJ8qEW2Big6wT1vAcHvI8LMMKCG8dfMtpO1qvHQlrFFjjPkbaQ9GpZEm7B3MuqxPmbRLiimzSVwTODPKjuORPwN%2BkbrC4RcJV44FaN10UecL%2FA32fEEM9NMMwx3jztMmZyj5HO3ZNl%2FWfIfdkyDdUIROU8P%2FgkXJrozK%2FVRF1UtMRjolFUv%2F%2FYHSCUC7szA1ErKdlIQiVE%2Bd8PlpRc0IOPqgV030md%2BpR5y0s18yFr9BorVKIo59VHwt%2BsxitQK7DNXM9CmPYisivzXNrimXa3WHpbg5z7SN1ZQoyxYMmfZaONbQ3nmvgM0Vb%2BQyMkzBWmano4pGAd1Jkqel5uPgvDYlS6JaOz7T40BHoKS2yEcucevdLST%2BS9RWKbDF%2FWhedMvrNy9OGgEW2TowXK9qjYB7y559DbkFxIBBkfvYtl2fehz8YI%2Fm2TUxe78%2FBuXhznlJ%2BwBU0xOMvJZPjCr6lr90kdE26XUmFgV%2B%2Be%2BfCu5t3jPbY%2Bal3k0rAyKxJZGjSke5Kq9LbSO4BGkx7vRIJB7TFqooHfqFn68Du1QGYOWONfPyzciJZcEmVGaexDbZBMMb258cGOqUB8XCzDnqRTKyD5Yn%2F0glDuLW4SQlQhR1Qz4cFP5pVhl5uBagUrvtSCZBiCJMppUBxHxeAwCSz%2BZPr91FGPt%2Fv6H6gJemP1lhAI1brb%2B3xJBiGh%2FCYRjLFipO4Jr3MMU%2FGtL2wNvqoWiNbVn9L25PTfmgCGvAL%2BE%2FhaIvIcMsFLCf0TSYN3U4tpTPybh4w9kprLySjTNx1YikxnXLnWJyEZMVxIf6N&X-Amz-Signature=e6cdd69855f95ef42ff2a3ea84491069a6908088bdb51856d020e8614f9ee36b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

