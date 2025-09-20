---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YDIW47HF%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T010041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJIMEYCIQDtFgSuiLMUozH%2F6ptq6WgdjvPe4chgl1KRIKkU0KFqYgIhAP0BoDDH5%2BKK4KhqybXBPH3Lv1Hztuw9%2BOEWPtL7ZZJdKogECOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwtDDTwe%2F%2F%2BNes0hzUq3AMNAAwMwjGxAfX63YEe7E%2Bqf%2FmbXvXNJIOP6wQbbh94HcF4ukgLTsJ942peP9CaveeV28VQiM6HiU0z32VblJJuZ%2Fv8qCtv0U7v8BDZEKWm%2F16sIROJ1woYUzy96mNHAYICoKXqFPdU%2F3gRlTtijDxIn7bSj1j%2B6P1KS2KCkxpPo6A2NUCofDBV7oPhk4%2F%2Bo2EwsSaWm7vlHjIc2Wo6G8b1CzbuqVYqAc1WUUpnEp%2FeCqo%2B7f%2Buouw4i5hWmdefBHUSRRHDgYGc9BwNAdzeVMyWRG8dr5a7AnIimSzZYl%2FEYbGPIbwA9nFDt4qMT6GEDldPicsazAj4fg%2FqW7CC%2Flid%2BwOAtNZJvPx9tJwvTUhhXI1Gfw%2BTztoZPY7SRRigygIKvHhNrBCyc1vdchHFk0tEFkCX5nxDFr4%2BvzcAe6Yh8ez6K8sVrAm7VoNwkfGY%2BppOxGgIALGBuAdFQzL79tXqWQcsY98aDISP1HQSfXXJ8rcIKEqdg9H82CoL7pNHEN1h5crZ7WFBQ8swUHaLODjWD42D0y580PkTsEaCVdl%2BHHek5bbQPu11vGBdxOvnXVD1SkEHtxc%2B%2Ba2SL1Oj7F0E7NV9wJJAkDB3xld62C2S0AeeUe%2BIRZ%2BxtthaojC27LfGBjqkAQVvPfHensH9%2FQiddxNdqdtKYNR%2BE7iG8D9JeYDEfNqUsL0FC3FDSwA3KWvbEnHu0t5W31W1yyAckLVH6dwEp%2BzZqcjhD6khYyZIwAkq96VuD1huKZ50YDELlOU6HCdbr%2BQQuCmzeUrUSVl5K5OZ9ABt3BRL3BjGwiyXQaPbULxeJx6%2Boj3YNJt5c82%2FAZmaEL%2FxTKRHMnXFkH9MAb80w0CIihEQ&X-Amz-Signature=94f4a68b187f24b11bd8c624f4cab908c3eceeb3ee1709aa1430338403da8a99&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

