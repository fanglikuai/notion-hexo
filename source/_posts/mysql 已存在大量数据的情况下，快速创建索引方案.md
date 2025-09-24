---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VKQAHMOR%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T000049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCBfcTl6vpZ%2FAjx2NxNGWHqyORGNFRgKNIL1L0xThZuNAIgASScDceXImv7Zuqo4NUqJp1K0j1x0hJNdDrSjBSmaC4q%2FwMIURAAGgw2Mzc0MjMxODM4MDUiDPsl93u9Gv6wMggBYCrcAz7kMmyd673LEl5MCJhMpvckBqWnr38R3QnVj8qsyWi0RhZaaXYgMyu76tx6q57p%2FEF2PU%2BhQvesG3NyE5xuMLReFUNvipvlfmVoEAjC0dEJLi2DZxAN%2BO81Vb80M8VPME71P6tA1DKifrbz6eF%2BOUhGy9Ct%2BKMXAfCLLgM7f%2Bj5YI8I1gmZFPKspEWZYZopI7F4sdKXTLEoouHOhckkVZooNyxyD%2FbwKTBNgcUKGvYxNTWf92CHs6j7NR3OJp20j6T0TYMkV2eHQ4GfGMbQU5NzqZR9c%2FmRJKH%2BitkgJCcvY9yrvK8jSX0RC%2FngSUMSuv6kSOa%2BFWdq4kSyAIc3xEcdAY%2BPd2pupgC0w1FbIM8xjLzNvPw%2FjsnQccB%2FEJfFnJ3iAOAoDYaAdJxswi3PydmxVVNnOe6DmFKQWddq61ZGF%2FHWWWkHig1QzIsCZ0oZCESP%2B8doO357Bf%2FH6p1tw6nxxXWIqbZGuyXwMBZVMORxRv2r2xe%2FWJFuKnoyyXUji3mA4QwoACZmLP002keBCuAXt2qTHCB8mN8RwRzQnWMc21%2BBgkv2kmNBtPZZenXRaHhHxro2F1SAKixQYist4boufapiSeyxGnCaztLDbYWCSx5NyshUQ1kdCg8YMLXnzMYGOqUB%2F5IAkJIIenFyRhpj9IAUVuV%2BXB9CaGdj%2B6P0%2BaxWKKaw9L%2BnYtCWYvQHBiPk2Pe7kRlAUjIIKAC%2FyBK97I2NP6wB0cnRVqsXbwZzj02xtBf%2Ba0ymjdGmErecJiUOZSHgJ37TLWmI7vdHry2gYZsEzXAviL6RCfBvZEVqZnZuMaUkrJRWNrzSSCluqdjJuIYErhAmwOSlAWlnoJ%2BjrejbW0Y%2FHoTu&X-Amz-Signature=3dd39d47666c040ceb23bba6b63f5bdda18daa258f25b43daff93b35432e7823&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

