---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WFQGH3Y6%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T120047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJIMEYCIQCkhnhxATd99Kz256e9FfQh8Djc3E6fUi4NIKpE%2B5JgFQIhAMef5be1jrKZb%2BMK%2FZDWh%2FxhX8F3tvNhjsWZRCaVemzCKv8DCAwQABoMNjM3NDIzMTgzODA1IgwiaCB9uDLmtVmhoyYq3APmob1sB6GEbyqY%2BqlaIKtYP2FpEF0%2FE5Q1xl2G4HjIqTOEi5BiudNYE8VERv5lNY4tuh6IpPctjijCeSm%2BPGeCVmadBOdSirciXhNXFQWPdOezrJSLNAc2IlttiCC06HBO5ajk9ggN4FbLJqD9BjS%2FvKdd12KEcw0wL93s2r6soXz6oJsRJV5rHAGQ%2BKrDRwJOna7DuKwu0ccwVVMtyG2u6rmUGC5sZYBQeeMGAL97A2MdU%2BM%2F0GCK2R77TobduYxkoy7ChNecAbu%2BB6OQBJAO%2FNuC4TQWJjz6K%2FSiElrr8ZNScK2T16pPccRqfY5WmPafwUMWZHEmLASXzJDfGl1BoWt218lk2PeeofJDcQ0ldbkWu2ve5uzjyWz%2B%2FQe%2BNoItVJYY0l6MbOAAzl%2FVHK8cEUEkG%2BuIfRRdJScNyTxKX0ukYoaaK8iY1eYApHabvw9bZXAxqNkPGpgM0quLjZYKx4urY3%2F%2F6436TgxGTGs7PQpMBjYgmos8vOyJ3T%2FhqVZQZPAillqiYjx7lDUyFuH2Hew6vPUyslIAphR%2F7eLz6HQ%2F1ikCjnBN%2Btw%2FjTyoDfM0s41CSE0wSQY8GpFi%2FdJ4gByoZm5o4TwaQtJ2deTcy39qrwg95ko4ePBIgzCqloHJBjqkAQ1tz9fACMvB3SPbqb8oEFk3nxNnM9q32MF1PsyE%2B21TZ9GcM%2Bh7AYxX1DLC7OmQ%2Fwl3LDlrGlaJ%2BNvPQjw29xASiPnh50qn4D%2Fhs3Ihq1Qdt0dH0l%2ByUys6GLJKsA4wodavwgiNrcmAVVi0y8Qc%2FlaaYIp0q5s8hv2i5ZTpCoYmTDknkRd7uRd9%2FYfpOH9M%2B5LZpXEI24pD40QUTuP7iNqXqI3t&X-Amz-Signature=80ce5e6b5f1768050ac389720cc616bd98c19aea6b33a1a9f5a8a30d973bc9a3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

