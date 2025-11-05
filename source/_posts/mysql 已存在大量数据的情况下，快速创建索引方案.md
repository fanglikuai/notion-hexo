---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ETG4EPW%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T200040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFAXg55j9PbKADkjwz4t4zuxscn2%2FALLJQ6TLK6RJgCwAiB4AwNO3Tlx9zSyo8%2Ba8uNjPN9me2I2cJqK4FU0k4t07CqIBAiV%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMukNMZxEEK5lJsMFrKtwDjg%2FUAHcJuF%2FSA1RJHc4g0VsCfyCEyQUsgW1yvEyd1oKACAdF1kG6EsH%2FHBLxguc4fg2fje46oR6m8pxnz6vu1jo2p8WUs6pQW1MqkAJWo7OlZZ8J2vXu8Aefyj55cqLatFbD9%2BJR5Pr28htUfCrIQOnz6zHU2bO7x0sALYUocK24jDRro6Li6cja9sXQkASEmJsN%2BWINrNufovlqrn%2FWmrhU5K%2Fx7%2F0PMqfvvrOOeP1vddRlFzZJyxc7Sz%2FDMYdEIE%2Fh0r9EA%2FDZtN8HIDc1%2F5yT%2BbUTRZC6z3ipkf5ft0jTiZHaj5jUIPO8LSTK2va%2BJQgY2jFYu9SIw9jVamBDbBK73bJ1N6q0snIuCsDypXBCpKl5McVJLj4%2FqzO969F%2BXqh%2Fe%2BUqXu6JDaSry9819u4a8ds%2Fvn5DcfXA421saaaug8yjKoX7a1VxkJHRnCi6rwZb5%2FqdtQMYMQPVWvX%2FYgo2GRmBoI3Zn%2Fod6PESMGZhoY0IEAXs5y4oe7obOq4pTrHkPUVDsV%2FxkowAm56y4%2FPyhKWVVweWqXrhKQy6OHtygj82pnsIFOjTj%2FesGwesNjbw4VmOjlRhNgwah8irDYY%2BZmlu3AnwAFmzc4eT6R5McJO3UZK8HvkL4dAwi8muyAY6pgE%2BntSoPieDR6%2BSP8GDRTqdHcLLHsB8wozX%2Bv3LwCAhNVz%2BpbCfVVxFNKyLHWR7T1sULR%2F8hDsfidk27SmBxP5wWb8B%2BlyKO7oJz8yu0noaeMfPymLRo7TbjSSngXtL6a3bIHGTgIUadeI0J1uId3T1iy5HcLui%2F4pk1htgszXTYSywHZj%2BCLQtHOlu7JJXAj8%2FJghtv7NwJsqP6Yj8D%2FgwpKGuZLv2&X-Amz-Signature=e916852467480f97a8568865f1e78af6caf2681edd3af3dce84901d40acd27df&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

