---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XVGT5AQD%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T200046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcZDoAkkDi8YifUHBxCNMYA%2BjkT8YBSdQtAzIOUVOXNgIgJmmWi%2BsYCsa3Mle5u0y1UyTGQIY5QNkKLT1h3imM1g8q%2FwMIfBAAGgw2Mzc0MjMxODM4MDUiDE2%2BFL%2FQXxSB5UB6gyrcAwqDObrFnePvKqHiY96b2MC865udqCl3ldDmyImdhEpcirGgEfuBIqi5QAxjqnROWkohiM4DvfjLoKlbm0%2F%2BkQp9D1LuDkQKi7H9ORiaeRDhL5U3xAtjCAHgjVWY9vxKnSJVXYey0FCDYo194kO95jlFN1hjCoF8%2F0J%2B3CENhAWTMjjJPw6gls9HhNTS5g9ndIxeORsdNqR6rwOfW9DRGvRuYmvE5264QJEhIsK%2BC%2B5ELLWPtV5U76JR5dqnaGFWhPf1jJsxXsXGqTJ6GP2mfkVIXyq2TBBjZy4jqKpsHNeA5nfVJ2RY5%2FvefqweXjHxhY%2FDvsikjOkm%2B5x7iNMDiTu3WyWBXqa05LEfqHKpTuN3RnDdABJU3FDV95M3kQ33u%2Bfcg1R4%2BUbmQpGg%2FeSAH3Ht1A%2F92nND%2FnniLMgdGpHY3B5ehnmqdyECWNrVEdTc0tnMZXtb%2BU6zQbF%2BkFaL14fVMvOGla4xnNghgEAJV%2FUF%2BtANMzKAWL6ZBedlnsYEgzxEx1nL4Tr19l%2FL1x55%2F2u0fzYwKxfsDANsCJ1P6nRQtvQNKPc9g2tnmM9XsaVv8eKaxXZaUC5g9eH%2BAr5L3EeEGVp3KBcFOxDued2oVSWuAMfz5ZjJkK2HUWswMJqiqcgGOqUB3C5nfJiyXTuWZI9ndUuK42vlKbiWQ%2Beh%2F9ls8yaH1pN3feyIYmgzOKwO%2BS2F%2FrSsD%2BakELk0CKnXS315Gm8b%2BFt9ZS7OT%2BHJlXCYJgtZTXXvbZ0r%2B5caIG6m740YKB9l3QK6ftKskqn2Btu%2FqgLrBSxqGtnfRPezy1p4zQpUzjIMvbFGmNeEByC378HzcqlIR1bNS6JArpBULzJ7UZTmobplkTgF&X-Amz-Signature=fcef5f2784f14541501456e3adf95a25a70911ba22132ca046d2dbd29158670b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

