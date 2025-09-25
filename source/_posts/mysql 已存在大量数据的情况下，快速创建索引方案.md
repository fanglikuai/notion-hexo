---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RCVEHHJ3%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T080042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEVupvX47xv7A4x9zkHTDi%2FRkJgc9bI2nV2IgnIVUVp%2BAiEArWXRJLkRNz3lTgQOQpaTHHNAW3J%2B4w%2BQfixqfKXnzNMq%2FwMIbhAAGgw2Mzc0MjMxODM4MDUiDMRF2%2Bxp7O4yljYgiyrcAzFHo1u9hc1MDmf9B8TBHxiHmAX0oTF2aGfSRHf8R7oZPub0swVliuv5ioCdAhrP%2Bz8cl7OiyT2hY%2B%2FRs3Fx1AMpbNhEl8SMoLIISBAAhLek4gfnRhzDi3vgHdj3scFRLKG8w7fXBfUpQKoJG4V2nUxRVvBFWFkfawiLVuqKHsRN3YHLdmXTN9vbxIB93TQHiJYit4tCtuw8q0mPVcFM35cPC7sUIiJRkPTTCoWa7s3NrLHZb8OpgGH4jom2FFt0NJFBwAdsFNj8yzF1lA2APpGEsd7tY%2FoxD93mTzMEHdUhNHC87ulwABDRaP8uE4pGqRO8rrOfxPB3dKeiocN3pTZxiJC6yoGx31KJ9%2FNtQc%2BUpEHOCwEGy%2BlhdE459sOWQPHpOdqZ7tI6hFP%2FJqGTQ%2FAyGXxDy7%2Fr6dySOaNPzU7Y7ecd%2FPxuBtIqxpZCYUyHzAYO8blxwtcSIAtnNa5PIX6lka1uLzPOq5532om5YiCkphKwKDBNV%2FMJw6xUIMInGMVW4YULHjpvHkbbuuS8mMXgPrkq4cmQK6km0mQ7ash6iH%2F6%2BQUdzwA4mtNLTXGdBkfuVpFgV78THKgVPmuJYAcLIi7UlD2j2sISnBJX%2FktGeeRx11PAlu1IsSrBMMeH08YGOqUB941VmpyoolnihT2SWFnWxNzOLy4yUfHd2o%2BnrjFRl%2F6QJ9YAm6EGDClb5tXOIKkxhlQiN5sM7r8uIqBnidaKWZXez5JbtR70FXGp%2Fh1Tyx9z4TGZlrzVt5viHADH6DWGY7jXxEcudegs50M3iRrEfisDriy%2BcXMCmnXpjqn6sGX4Rsa3X3EOwwUCuZdj4Gm6rmlKFeHKOZg20aUro%2BMxpfRJAqdr&X-Amz-Signature=f56016bfebb36193f495571ac8c180f8cee60bda868d1d65fe2b594760f5734f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

