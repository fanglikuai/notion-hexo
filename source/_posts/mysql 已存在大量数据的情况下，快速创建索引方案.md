---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ABGOBTP%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T140043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH4aCXVzLXdlc3QtMiJGMEQCID16CfREx8QZ4mOQVnZUYKqtKKOD4xn%2BmjbLBERRQR%2F9AiAY2bSNhnQc1jU90jPi2CLexvFq7bQjfENsTlgJb36L9yr%2FAwgXEAAaDDYzNzQyMzE4MzgwNSIM6yngeFXmbCCoyFDDKtwDYJX6wHgOvqgDdZE46LR9JrPmjfN4PN8rqLeRwzPv290o9Dy6y4Iv9xqf%2FgVX0kJaPNZ6RXt4R%2F6kpCfsJV2kOSNs2B6hDnbfqgRM9BY3dNbHCR1J6ZirWiUhQdQmCQFeQKs%2BUi6RYWiwfJHU%2FGnBTaR0aElz%2BLgJL69ceS9HVoiDhyu0eD6UDz4oRYD%2BrhSJsSZvfo61hI0%2Bj%2B0H60F7YhDQkEZvl4G6PmvIdU1wVxu8KtN%2BeVpOpz2AZhGSMl1niqfG9hl0z%2FgYUy3Ke3bEGPjUdejmJWt23ByEUALHf4CwIZE%2BPApNX%2F4CjJQSDBXdx7HKI73o21fc4qh7Cu3QC5HhNEmsNb7r0Ky6bJvqt%2BFGBTsaqa9CHEDwLSmiODiI2GFh5hfCV%2FJCWH87FlDlc%2FhX0gPbALo8OoLqob5wWNSocAifUZ30QIswkhHHcdwEM9yfsyhWv9jHa4luM5hl6fqFQqakd0SuO%2BRx%2F7pN8vr%2FuUVHcX0FAF1OD7Xr0pCKk87144wQhLA8J1XIS%2BEg%2FNkmdkpaRyDb7j95uf%2BbipLslwP5ebtZUH%2BNedY0Qe%2FettTYD9lM25isgjY23PSDda%2BPwWtnkdRGvf9kCiabhOpJVsVVxIuqfcfSPc0wnOL0xgY6pgHzRikk9vpaajYs0yqTYqZFnzmz4HhWK32lAdd%2FT9sr3h7GqtCbBUhBelm9h1%2B8ohM%2BX%2BZAl7UuJhfKjnRAbpk9WL5cfbDVEMNlDxhIK1lJeXzUcVWqyb9qw88QSuAc2KA7ZQfSsWcIKjM2cIw%2FY6y3adykUSueNoGVG9brArrp2KRHQtn0VUpCFv9kr%2Fc1H7c5vYcArqi7dVIs47Tf%2BQoGJHMSFy48&X-Amz-Signature=fa5b8bc26a0818c3832bd724237c2b83d73cc92dc45aa5b1cc2e63a27275b02d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

