---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664E3IEFPJ%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T070046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJIMEYCIQDhx5OgS7ESESxOWaQvIjQDZSzA8htN8x6o%2F0VkHANrKgIhAMRg%2BU0rRPxzKd7qhL5d6k6nHk4Z5%2FAPg7UtF3olNtBYKogECLf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwyBBMmHTW7Tg5AVjkq3AOuYwd6CWsTRATdxZKTBxztZT9Osnk1fPsAidHjQW6e0itUrVf94P7mgSbnxeGfA5nr4D%2Fy175JqmUvEQ%2F4QwEogPEJnKIm4HpUt7BQPh5weRqGcJvj2o2TQuPvvz03cCsYH5dH8gL%2FujOjZCyOC6ns3ELl7cEpUMerIuQeHRyi8LT6gXL2sAc7zcHwI92UzIaOEHbwaNoAxHgud8x5QWn1anzgVJz7czL6T9jmZltFFjBOoJ3MFR%2F%2FwkZHMTSLR4hWrmULLJnQpqMHiyVsBRcrdzpyqJj%2Ba3xqOtEeciEyE6%2FBTsFszGr53zMPKhoWWcR84gYf4HRe7SBWZcCn3BMFRwgiKV8dGrm2cww3dNEi9LgTfCU2XQRGX%2FtPu7xoWF7tJTQ%2FOn23ZguzGDJrXgopxmfg5gatLIfKNPgaXVAJxKW1MD3D5RK9pfOgfdV6qbELN8zgrAOzayhIJ7nNbHdQ%2FZZ345AytFDgMZgvR2bvoCFcsiOZUCk66a1pz9RrF%2FQX8uKsitM3OWR4grCtJ47ThqaWsDzhj%2Bl%2Bg1X7VELdFR1yABloq2WNM133UBlqWIx%2BiuwQjyxXkPy3ammXS9sZIiTZWmvigwI%2FKewT8oGgGBAWX%2BbUH8JKbddH6TDi5MzHBjqkAYqedhMlYKOPTmJ1FD6dWjcwU0lPI8ASE%2BonpBr1PL1lu%2BBSTqmXveCtc250HypJF51WuP3Cqm2C8PA%2BCkDn%2FbOQN%2F4I%2F2I3VAyiXc%2B46YRMrnKFpgM3zlLEVcJFskqMwcmmyV2Vrabfgn3qANTOoK7b%2BQf3NhXUiVg8iMV9u%2BHxyQyfDWxFfl1eTGwdDr6zN8%2By1uvQhgfvjc%2FwilHeQH3mW3cv&X-Amz-Signature=bd777341bde14ad005e8dc6e014c47fa26977e6fa9dccbcd93927136ec166d5d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

