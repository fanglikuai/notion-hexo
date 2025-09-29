---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WZFMJ3JY%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T090039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJGMEQCIAD%2F9fbrX5xvXL6t%2FnNi9dOitg0%2B8Bvvru2uW9wj9cqqAiBe6hZnCyWPbvmuQP4lmAXPE6lZVCcHX1yStwn3alNcfCqIBAjS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM2a5FqZg%2BouAVgNxyKtwD8xBMn75Z4Cw5Eoo94k%2FKg3US82NCSQHYN6RMdcOHO2v3rhzBVpvXiVyv9hBMXw8bIrSzrdjnPk5%2BAsc4CaITOLXs25Pj7mrfpWuxMkUa8UdGclnLIlgnAPCoTVOIPq8TUCLphy%2Ft%2BSB8pEL9Kl60zXPB4LhOIGD92RbLiUAWep0G7nboJh3S2fTTyX11%2FNIfaHm4ZUkugjFd1zK83O%2F%2FuUnTzdAQqlF2GwrJFp9rIRsALwjurvI9a1J4fy8x8qC31tfCfjM2kdhhCvglWa5c4U8l9q27l20qssGctyGhniHTe5VUg1lzBH9JXpYyMo%2FvmAKFVjiuizPoAfLl13xY%2F3XaYDnEk4Dlb1%2BnCjAaAcCtmbfJhA9RDdTJeVNQAv8LfPjL3KghmocK6CSdkIBsf3GCH%2FmC%2FdNZjk6ShebhvE%2F%2BPfCTFEdtGqrJnZ0OVK%2BmrrPKQjy87vwaxCSoZXGyOml98eIVuCzCZclTXvU1Kvn9Ih3BganYhTeD8xRU%2F4%2F8eaxbVvuqkQaJeleeZJoHvSJirtdfj0AXt5RI93gXUDvK4Q9C7SeikhYK2SiN%2FVkEtuKCSVu3yTvHVy3yCi9ygEjLWNQmy7eTkwcZwWwQZCWgrmleDBqlddF2274whpHpxgY6pgGQPl7Cju%2FM0trpdw5ahaYKl8DJaIEsh6xouKAEbEgBbGOAiruGAD32n9fgWaQB1KceUrdOk%2BSB8%2FPuXOZzbk%2FA15w5BMg5SLjPDlK0i90zF0todYPyJHKL5nxmYfDMEvppuy5dU3ud0nqQR5r7Q%2BGYJCLi2UGPkYyEZWJA0P6jONVo2NJWmtQRUOGijF7bKMseIkiPHfIOrJuc3X2F36HonUah8In7&X-Amz-Signature=f055e88f00cd4c669c972c9e7e245432ae6a6cac0429120f964628a9636b332b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

