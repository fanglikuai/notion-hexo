---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SVODHFDA%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T050042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJIMEYCIQDJFbq6J%2Fr3wT1Lw7fzamNUwlgLEeQ3RMYKuINnLEh0tAIhAJxykXW9qi0RY5d6LaMRXZCzvgcfmUhzIKqld7b9Cua0KogECOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzCTFqXHThoJEG1F3gq3AMzdBmKf6ZJkx%2FCDhoCrPIX8BHctRJLpiUkNp0aGTrtqXu7zLR8gzkuyCFW%2Bg%2FmdL0rhD0lMWQtjvjrclVo66mazuOa%2FnAMftfgPpW48IWEuMGBpbS%2FczjV8zfoAri5jmoLOCFbWHJItRT5y%2B2ioQNlxfXJOlwsTP1t892I61Fzc1PIlcUq2%2F%2F0hihLrJt2%2BOO3Oe44gsqivNdlSxKy1WFtX8tsm8kSQzKGuuhrPSfA9gg4P9rV3wznT90jbABNTeREwoWgVTEORpiwHNBBOUfbvECZJ7lw%2B568EgQAUQ%2BRqKjb5JjL3XpC%2F8QvnvTSvBHApcf3i%2F3zK7xE02NWLEw2biO5L%2B1YT%2B7pXczJd6TqTcseT%2FXTDlD6w8UOA51evMjzandRM7eV4XnW5Ixmjzs4usdFxTZIpBGLrtOb6JeAK0UBinlOZB%2BmTkFyw9DsGbPGbL718nee3WhM7GUID%2FgQbXwowLJSKlZq%2BKJgNJhzxd23WV6dfwoNesu0P8JX9c33QR0C5iDk9JErqOykS6pQptjWjyYHgkM5WFAeMY%2BrKB1n%2FKydvUCtl%2Fvs5BBE7b3AqCHEhxH6VZFlURImpA9X9SLN48gpjRW4QNQEDTqkLgmzWd0HglLGOf29tzCJnKLHBjqkAQNocUfKdMu2SBSS06G7KGP7Me5zNVPh1FEMsomV%2FlUcKMKvU81U8pFJrNU4nf3LbvRqONwoq9bn3NLP1NXxqhixw5ItwEVFMgO9sbxuRbl4dVJg3XAQ3zN2SzUo%2FaGY%2BY1%2B04b15oN1JUTFSMl5htwGSRrlNDdJ1JjnHsY%2BJge8dy2p3NXd1C9%2BOa6vBx9lE6w6tgqi7JTsLLrlajBuOoGzsfGW&X-Amz-Signature=a2abbc526bdd476fc09157894601b629567acec25ee2f96aed484b291c13ff14&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

