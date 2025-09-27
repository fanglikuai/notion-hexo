---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T7CGMXL4%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T120050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQCACM0Brxlt7fng%2Fn185Oroxhr%2B%2FBlDjzvlt36w7FxBFQIhAOy0VwPx3JZ3EMp8VVzLcwFF8KvCGm3mwcDfnYsKryLnKogECKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxYT6guqVF7hupMP1Iq3ANiHebzvkrYzDC%2FBw9RQLH4nN4t1mr6M7PDPD5eCQQz2iJaQbFdQO3dO2RDzYKzyKHXgOxABzAU0vyIggk771yoFpdshUm2L%2FfZxfo%2B1wBJprwFSYsnK1YEahdpnyFO2LaKSEkAWATHReP2%2FhiwsUgxk6bk9aXidRty3KTC4si%2B242tjAT9QEAIn%2FxKU2c4liJHHgZPtHY%2BxQyf2WGju7UZKvYBq1FAXEXV99Qr%2Bi%2F4wcoYYQhucLToLeVXp2GEFoGqfbMmHUgfP7GI08WVML%2F0SqWQ0WLiKShff0RZpq3RbPWFkre%2BKnUcEDcUO8pzlGu%2FAa%2BKdQgcc54FqkqtfW%2B1mk7Tx1LG9UoxJl8%2BWq4dtSQSF1cGvkjoWY8SXuuxdFHVMifKIO7Zs4iT%2FaJBJiv3cEqjhJYTTQHjdUQsUd7Sp5cCI4Vy4MyvM4pjC984Opz5KMK%2FJ3%2BFrdgeW%2ByRmN4lJKdLXCIQNRIIzulPvlQKg0s54OaxkOV4e0yVR2LUENNcsIF4AaH5JlwW5b%2B2X%2BRwqg3GJbsJdtEF9QwFLnkUmoOWpyrsj%2BhUGdDxND3zB3tPnYAqeUOXzr%2BQrcj3i9%2BSDMd%2FjXDM%2FpzudGy2GgU6xoCLi8BCLW4ByvIEhzC34t7GBjqkAU96sHdtrK6U19O4TP9cy8%2BpRxxKO51jipnbDg1JD00eB4ZoBLTp55GXQnuvpaD9c%2FCp9R8drbqoQcnx7dtCSDG5BhJyDQ%2BnNuGKmDW5fdJ67aLzkj07wJN5QFeeDbNKOZ%2FH602fKLouIVtVK%2BpyczzEICXEhwKjvA9pM1gG5FQlBqw2mX8IqSZ7jOWADYfAmgKqiEiYlwWcnNbtJfzrVMs7Im8e&X-Amz-Signature=7c045de718fe3f2897bb4619b011f3dd127c7d738808e3c8f595a58eb9059f14&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

