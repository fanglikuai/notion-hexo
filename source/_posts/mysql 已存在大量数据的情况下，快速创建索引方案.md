---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V7HH6LI2%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T180043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIQC%2Fh%2F9d2ElSR2Duhg1JbxzzAdEqJ04A8WHf97m68a5EaAIgKwiOLIx8h8hery8bM%2FteY2NPpQ1PCdlx9IpjrGBV8aUqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDN1cqOGa3MofZyLDWSrcAwiqWcnZWQB%2FnssKhb0cHQUBVrp0a4I4mIHHzTZDbbUQrRnc7%2FRodQfixEv68s5qssFYdJOU7K8DyjsoESrqkWnCMdz2fYgAMxQJVscsGBbqOuUxm%2FhxAbHESJ1JLDrFuwtS%2FEns%2FdthObmtvh2RWzVSZuSzYcx0cSqf%2BjeWdZJFf4eBoD8ndhhbMHyJBlGPAlaxlkrrepcy4wgnOxb%2Bwllt3laMZsd3Z%2BuZUZZ%2FXNSKXIe8zLUN6guvgasxmrprMcax2KDjiKGxSVu5Y9f%2FO3tS6MzYKzwBMWMLchcMNIPAaiuVC3yenyZTD7vVb7AzOuwOiAB9jQORJSYWoPq%2FamGvArsYIiDJfP%2B9utreZEzZXXWG24UylqjbrpV5NuAPCDk8P0uarzhinIU7sl%2Fx0U%2FLS%2BHBsl7AsNbbNaJIaa59VR1dCnv2QOFFdes0KeJSslMqvIRF6QJohorgm%2FZ%2F3VYIftHo0qOAoI6V%2B1z%2B5N2C49t7KVTE5gPzFeuKvRzEzhBeo%2FqsHF0l6VStuYVgjLkE5tCXwcRUFfI85YeYeA1qnQx6rliaPP2KAQfggAHosNkDXVXIpkcM8KQBktVzwfe9JLACnkm%2FW5BR0NKJFuiY6kabZpSWp3qU6O8fMLSbjsgGOqUBsiQbUxwpZ1G7qSv%2BkN49cDcjNWdEQFQwK2LPfYT%2FvjK9lSOWPCFJ9Ps4444LTdewXtYfyx6J4X9nidOdDxn8rLaxNPf01LMc5HRSSvw4AW82UFmYAUBBtIaE8rooRXKmHhcub85mQmqTSqqiBz%2ByWmkmPIIW3fTr7Z9d0t1DopoGuFzb8eNiNjmXv9IXeltWrWf8UVwvloKtZU%2FT9x8iFsLKYaJk&X-Amz-Signature=f9d08477e5d54789de84211e322408cc66cab2128242b58689c4e889cac4ae03&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

