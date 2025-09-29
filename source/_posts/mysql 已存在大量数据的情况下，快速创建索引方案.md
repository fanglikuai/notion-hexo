---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TGOOPBFB%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T190045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJGMEQCIHA6RYoaYBNwk2g1uyu9S9jrN9EI5JscyE7nd%2FPno5jzAiAYtoD0nUjbLa4ABoOgkTyNz415ytJmf66nT%2ByFDc4h1yqIBAjZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM8IvKmjfSYVKRfyDhKtwDVObU%2BiFSlHGcpA6R4gzdn2bfDmuKlAf8HXMMppSGit1SnyUUeTILSnDInRq2w8y79gPNTts9cDVaH%2BCvxdne1kgWfXE1tKg8U6fHSomWvjGPc7XBqr1xnUFO2oigxGi3cdOzBA0hE0TkP1m8XmajL4USnMVVUCUAj2cNWPT%2FEkRDke4Ku3%2FtOtwDiwWRxvi8PisyL7RxSL6G9GjwpXS0Z%2FdDLPrKlW8t6r9ppFHCBwywe%2B5n95ijC1vckF7FEvxsRFXc5eTJ3pNl%2Bs1JsuWV4igHY0b3AKW9q7ePGaQNy%2F5PZqdzyzEiwMYcVKiu0bbd4bHzGeznHL1sZPSUGbFafvIgDmiiKDnpSbaEojvxkHb4vGNbPqbtQdHpj2zmmSOKpK2J5ZXa1iyn5%2FamX1Z4Zw3ZKsSQ7rA01ZDVr2lVnpfCfrZLz4uNdyHQ4iJMxShzvd8jSoQjL0I17NgI6fMiVrGxeiWWY4sVXjohwzWlCCyuMn4sJyPj2M5TFEOKr0Zs3zIsH2zE2%2Bcd%2FYrj%2BkuMF6uks3EfnnD7zC6nNxVzM2ewue10Dt3zkl58ZBE7PPuUcCZ8SpBNUob%2BzIEYHGo86zahM3cgiG44hPVoKU%2FwnU3FsZK5NooK7ppZzTQw0tTqxgY6pgGok1S4Oh%2F9Utod%2FC3Z%2FZFGNHxYYEyJ7XSTUO1jl5nRENNYRfrelWFE4cgsq1PCPYSrZlEbyZGi5FKBEIWK8PEMhjuhHbjiuukfbsMR%2B7563I%2BwhS8%2FYUnDLBrrzVxlJx2ZO6hckCIEsfsrKLwPldackKozlTYXomy%2BjW3IyFc92YpSySqSMZTzmPVafCWmn%2BmgQFaFE14GM8PNsCYriAHEmTWAtgAu&X-Amz-Signature=8b87e1a8c92644dd6a842ad76df9d42ea395417f36f34a5e974eabac4df7b07e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

