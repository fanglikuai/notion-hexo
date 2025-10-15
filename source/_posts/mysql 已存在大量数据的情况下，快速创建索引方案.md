---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46627ZQ3WJV%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T170050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBy75OJzvjmYHL%2FHrC3SgzRUGsyF1NQB7iyRI%2BIeXQyFAiEAvA3Lxl6Fc5AFz5x0IprQa%2F8YrqDTR2AffqRNddflcvUq%2FwMIeRAAGgw2Mzc0MjMxODM4MDUiDIdT89ixBqY0%2Bubk7ircAz%2BnwmLAzDlkI4F9GJymiA1F9TJpsvRMUJs0leVv85m5zGnTwUbF%2BClavVHClVv5vhcC5OY6MPFPeXyhWQJQXrIBU36swsmRx0SAz2Kfw6KVewLA3fFMvM4msX9HGHicrFc1Oxcb0ZWlwFgQuU7M%2BZs5wH2uXEQ74Ah3fX6mJQ679aGLG3AGJRuQqyVN4exh7k%2Fy%2BSPnzenTaJc6BrC5opmy%2FsAqoT1yBxkAwYnwi6NgsYnODTJqsYX%2BtZ7JB8F4BQtw5qFEc8EdG9z%2BAoz3GwAhWLuTm%2FEIhgcfsfWGQ1Ttq79S120OlFkF9VEIBAiBI6h1sizwEQA%2FN8uZknAQnKl6xQPN2tzZvwPLJdo1JhTQxkCtW%2FgRl94%2BDweuoAECNeOfr5pb%2Fr2Joqnzly%2FIetImYvrpmveSWI%2FWv50LccCXw0xZV704gUvlMFtl7Bbek1nSuU%2F%2B67Bx3ZvhWtVcHOqrWyd58ZmX921whsEKifv58woh9Gb9VARe0FGH%2FRHG3IR10NqP%2B%2BMXU8UcTE0rqciblD6WwqFaxvWrr3gCbxV2U9PbgeS8bEjLTRl%2FD2FW7iFNmmJrlFQb%2B7xi%2B8LjcQsuPfOZQK4lF%2BYj0l1rBQReRXxwTdqVppgwiYtgMISFv8cGOqUB%2BiOWNho7VgZWKlAjTXrf9Fe75lC9Te6cn2Lvo8GIvJoDaevptoqn1C%2F%2Fw%2FvG5YNJdrVdhKvPD9gyP%2BJBz%2B6ztiBii%2BxMTy8mVY5q3nITbwkEs29tsEcIa0NbzF%2FebqVjCZVh8dmfdSZQ3CYGOGVDNozImavkHhIDTLUfRtNWtxO19sB0TFo0y8ZF6c2PeaCfgJ9u8p7qFXosRsVqEs%2BDTmIAlT%2F7&X-Amz-Signature=c2f73a427cd9cb2b0a5fff330c66d7a834cebc566a960d21a551c62380d9415f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

