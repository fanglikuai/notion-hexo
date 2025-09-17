---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663FDT2UPL%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T230039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJHMEUCIQDnAhyIks9GaygHFzoUOdMStwq3xDLC%2BPm6jCtOdRGR5QIgEXpoSrg211cf07oIG0YBOR6wnis9Fe%2BwuFgKlFvNk5gqiAQIsP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEx7pgX3O%2BuLQh9GBircA2%2BjSLq9F%2B%2BK%2F%2FEF%2B9pRQy6NV3ppSrGvE9vTZgCl0gizjqatpUkqmf%2FF21CejqtzVhhciZdAWylXW8ymk6kVV9gLOQJD21OR0mU4Fhg4bAtb6wrvxTXXD1AmN%2BSIL%2BZ%2Fqh1CdsaZXT%2BnIJg%2FnQm63KywXDjCkUzCtrylRpw1SiZtMKJvWT0jBPTrdweEqsfuISJDA3fv85AZbkpfT%2BfK1FkAj0oJg1edUn0tkAwv6Znv%2FXAj8CE09JQXEosqcHsN7I%2FNA9d1BJVKXbkir7wmtcaZSk%2FJ8XfWc66BabUagGKDqNyYuk6abNnV98Ixneik0nRY%2FydrQgZk2L2TtUpBRw1Ns%2FHW%2FQ%2Bv5FO6nS0bj716lMpdV0mjbNBTI7j5Rkit6kPjcFo77vUlBUL%2BMhoNbljEvW6kb6MSUOcMPwowNucqW3H6pPHiZ%2BJlg19vYOy4T5Y16YySkB4xLuIbITLIjIeY7OkJfkDqYXmSJimZVimBfh1MWIblp5o5STvQnv0k%2FmifNfE%2BPubZYSkaPUwOLn7dn0z%2FCxauZZcoY3g0sIvxfDSTRZ4uhmS5smRqpxYelJTOLcXS7xpJzgayHmJBhOMtiYDFUgYau4UJSNZ793yH7oGjs7zJDMRZTEhoMPz1rMYGOqUBFsVyuhM1xlgJaBnbAmapjJebr%2FNOTEN3GgSFe%2FzK149YVh2PGPZPFf1CtQCFXbMqo8oS8a0Cufe%2BvGEw00nitsidC1UQRZTn4QXa0N7qz4d9nADmTeeXAenvUnTGeZAsSJSYKhKwL1J5S9mucf%2Fo7YKLyap0VmjXVSKuEBrPQrz4i%2F%2F1xpJVPx5FyTJ3%2BDS%2BjokpB%2B3qcKUvSi%2B7SokGwegxEv%2Fi&X-Amz-Signature=b6ffc272bd7ef27219c49eec6fb60a6db530a3379e912e54ae0d77531a384001&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

