---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RDT4XG3F%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T230044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFYaCXVzLXdlc3QtMiJIMEYCIQDb6dgQbTSqGRVYtdUmU6m79uakrA689XuBAz%2B%2F1%2FwJjQIhAMkCeLHkI12uFfZ8eA0LPSG8sr5LRrrb2F00J3Ssx6WCKogECN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxlofEgfblPloQcLhwq3ANGFP9DiSBk8YJvB9%2F29jjtKQFYJ7AixqhUoJP2O%2F715No5pgXRHTOhrnrRLsaraCSFPI47paaC3KszTn6JSagO3ivRHpUB8pTL4QVJtprFCjuu6zDCbDoS0e1KnF6TYIWbD6hS8Gqg9odkr7t8Uc0AxXkOmI4pp8%2FSftMIHfTpJkkt5NXbwvSQhGii9tMwlyv6lE7ttTqRfeOaPmdIWCAiyWjAbJqZrTYUHDA6doSmEk16GqXeQOWKtrCP45qqx0A5c2ArMKPnOZURWEHgIc%2F%2FcNK15aBoZwIeuv4srYJC35qJuXc0e2jSs402F2572cUuPg5B8qmvyE8aJKLAlZbXMPhEoUhIx6aG6CiwxJzsDE2hviAh4rt2tz1vt1zim%2FdUSmQbf%2Bx3fHQTbmsXpK8qcZrm4FndsYU4DPK7E%2BVZ0ak%2FarwbEaHmSgmipHkVTLAJF0NJYWkWmsUezmWr5mn8EryMJ35dBA04y0Kx4GCKd1GDBFO2f%2FSdDaff4WTAGXKpisVKaqCSBRzKZIlB17AI5gzYkjBWSDtdJo2Wcj3B9EVtp0LUG9VEYwO5US%2BPW4h4NfwM%2BHUufwtI9QWUbqRE%2BiWrg%2Buw572QzeeS73La7aHHY17R6znDLPk5TjDkiezGBjqkAY3AeXvf2YlsJOzFUeyqBL%2BtjbT9fcETNmrK9A8m%2Fv0VE68SYAAMauW9pGWwtHLFZUrA4225HmuoacA%2F5d8%2FdGFa%2FzXzUKPD%2FcZOq9HAsfZcDCZwDZgLETzfz1hVf98oczNc8YKmWvlBmvfXQOlDbqV2oJSGHAvsH%2BP0vsgpAYJ9kT7B9S9h3otGcuTCXkugQHO1UMFoi3ro%2BoiKZvaOxOf4uAPh&X-Amz-Signature=d731340f1e0c3d073243618aedc0b98489dfc14c96e262ff2340e609c8333a1e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

