---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SCUYUZWO%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICic%2FwWBrsqbRavqkyUuUsCLAaRolr4FBuAeflOjoBXVAiASw3kZPcrX91C2tVTbNyhOa9eSoXTWVrC84XBLdRTuhyr%2FAwhmEAAaDDYzNzQyMzE4MzgwNSIMv%2FSzRIdpiWxaVEePKtwDSl4TkrHcYUZXZbByMNh96tB0k%2BzpjnH7vdBn13gXvvzBtSkR7SGHD4hYUbFj8oi6QgwLQOjibiBWYlf45lX1j7xAEdHdeBu8fPi3QFStA2dVlbGcZEhPtCQcFQ4OpmqT3EQ%2B7NurhqBuH%2BuhUKZIDN1l0VKr8R82kpajnE%2BGvmFytsdv17LIfzm7C8QaklN0mKHdNNpmvJL8tP%2Bm577rycOeQcr8dDNblmQwkI6vzjAQ3aztBR3rEEm8Kf16yWw9Xc4oXNsze%2F7e3nwWQh7ecAQDoE5ocODqxuf7Llq5RaQcHkE1To9a1QSwKxOAdyJLzvwXYe0aqlzeq5GhIuIWgQFIYCK1%2Fi02%2BZvCTmCdpDftPtX5iKxvBnLXxqmUHFQ2pv41RYeUZCTAM%2FK5Zb2cSOJplxctP2MvAPoOE1lYdX3gBmY%2FyA9LUNsmHiWHl0qpxQyplwnVFaGCqmTSgJhJqeS3m9F4YxpGRUWqw2NCeZs4HuWXKDJSRmDeLnKjoeDwDiA3c7%2F5k89tX%2F5UNfauBknv%2FfVRWC3j%2F6e%2FjXcgIAydCKm8Nb68q6x3YBAvoC%2Fdrx21ZB8Onupl%2BhDTnms%2BTZdKYrBNJ4DdgbBCO5jqYv54YbWukyjMnu2Zo30w28LRxgY6pgFxyNYZ6QOiIQu4uC1hw86wam9dUXOXx%2Fl0Zhtr8uqpglBV6Yi1FmAZG%2Ft7ekJ5k12lvspxSdlL0WFjpZqBGq2BCIzH5qy7unzPD%2FZawHjkIt6qRgdOaZEM2nFn4r%2Bholq2FsDFWRujjI8sryQct5n3WLYwnmpYBkS%2BZqzfMole5CEWRuu8dwZTFZDWHptvudUh72tRTPFthzVgtoddWj6PL8nTabnA&X-Amz-Signature=d647ce276f65728d7319cdfce9e1d4fe7ee57dd054a6206da72ccb3de5cebe2c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

