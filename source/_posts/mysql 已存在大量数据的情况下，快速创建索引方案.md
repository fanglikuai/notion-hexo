---
categories: 业务开发
tags: []
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XIOXRCQZ%2F20250914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250914T132611Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHi3lkeA1OZq5h4c7FJF6FEMy7yb878OuN%2BlvUOnnXQ2AiEA0TvrnB8pKNUptx%2Fl3mfkj1HaWqTLj8fzJQ15ybZ4Iosq%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDOuxY8Y2MxhTm0%2BEYyrcA%2BPhqJ8KrEpYxi%2BWUoUBSj8hx%2FzmaqGWxyNDb4iIyf1rkNdS%2FjrXkJvJQIjxQdNwG5z5BoVQMZHH3xVcKCG9Z%2BDUOTjSv5ONmHAoLZfc2m%2FG1vWUFHAZRFSZtzzDX3dRSBcWPuKgHafapHeACr1%2BTnnKIxpiJ2rRdP8ugEagNfJ4oRps3p6X2RUXXtFfEf%2Fq93KsA4KtqIRju%2Bap1CRIrFb9WCoMUjdqChNI5u4UXVPLSz9mpUOp5GFN%2BrI4lkcg18G%2FEhNody2zagjm4j1z8VamYSmlPiUq8VTdAzxngdH8Aoxt6OdgQWlONcFoFDSZ2BTcX%2BGm4kx1WIsnonGwSgfDBgLxqryqrcq8cGFE4CLMX7HZzbL%2BnpCgAILv588i8WK5AuWwqAVfY5XxocRUqoy3NY3PRMEwxgDlo%2B6Rx0QMe5HpOREpTbBTfdNmSX10wuNJBDPGEdbkl%2ByDXnYhatrZxwuwsLS7VP2LtPFm8sS3zP11Ke8A4AjmqYWHXJMxWUZvJSX2sJo6a%2FcZw9bhIfN%2BE0epL76BUdTvATaaSY%2Fw71Hpxn9rN%2FjapRKX0nW3qZwdwM8XNYjzkZ%2BW37q2C2D75FAQPWe3lwLgZdESGmEagCDKjV6qr%2BsFNH1YMJTsmsYGOqUB570zK%2FXIA1Evma3kmBYSzBAiR1hRX6jtbk8szCxI0WE80fgd1DhJE3yCDvmuzslkg3jCR2o2%2FEJ9nuFd%2BGqCBmBFKN2eHRDJpwpso2DE9Kb92FDRVmUUjCkh5fvNpAaPKj%2BufgN1jfXxNzN2bBWD3sMg7wMe5l9655uczzjN0Eva%2BEV1%2Bq41ZmpDpgA%2Bqg3nSq37mpQX0X5qo0Nct4FWYQgp0V49&X-Amz-Signature=fa09f90aa4aefd45b4b38e12f215c68ca07221404b22d90775b6c23d2b60091b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-14 21:24:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `<font style="color:rgb(77, 77, 77);">create table text_assets like network_assets_blend</font>`
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

