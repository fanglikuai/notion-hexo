---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YRSLPEE5%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T150047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJGMEQCIA4GKiGcrBW%2B2mXLrVq3SLY2cEjRF%2FC1%2F9xvznFunyyuAiBRu4s9vB0ETCdt8w97dzNb1MEGeVITVSsBNDA9i2N5CyqIBAi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXiR66kXR9Ld1pxz3KtwDGMAHKkinEYBpiZnadwoEFRurHSpENBIyxK9EUFr3BKISHEd3THaiYZSjGYLsesA7wW6%2FwoEI3qL8ceL7nsCeFVbrDKXymuOx%2B9mWOgUBBiWLJTjx3o3teN2cnSLW8aPepbXFc4F2EbfLDmJxoWz62Jq%2Fu0C9xr7hxLOswkLGmKOsQO8XWK2YjwEhQ6NBMni4w4TJw5pNcsuUR4nM%2BDln2ZVFH7w5uqD7mVA9ic6LL5At7TNmXKjZQ60JFdnGrKs%2BaOYvzdGnxCltHXrWkX5ESgZLDbFGQEWIDy0a9DWrvlLCyQwjzGhE%2BHmqSPW5IjqQXnbKd5nYCOkdJ19wMzMmI%2BaIcUO%2FBkdNS3Z%2B7auIsboUeqTVB0NTz4AykV69mZrINlWa56xffPMARN3BQYxMWOmaS5p73dEeqYY4yTOrhjExNsjdB74fl6zrTvcEsqzzdEBnrkIUc8FiN3FRNgC4Wlmscqs%2BQn7I3%2B4arX%2FWBDHD2oGMLUgZGxyWv%2FhoTGcH3gaJ0ugqC3cK0547yAtB1bKnsNL%2FxZk5EdMQkupcoFekdM5KxF5%2FLuwBWLSI6rXnegQHSM79OS%2B0su%2BYaa7DPN7kKKEuCHTtGVQcktk%2F1mmZqpQcIL4ZSy2irKIwv%2B%2FkxgY6pgH80Ius6RhiX1MbIsJq801dZO4ROitn2edOZ%2F57u55jmYxYtwXPMmDiQF4Snas%2BiL3fvwtsGfFyMzy1symZP4%2BmBh84lHwwTtQNiERcO6e%2BmYZXouHOcCYaRAEEP0RSZzOXZZHI2ISReSupMJwSETlGjhVP%2FLFAEGFxsJGIWq10U8TW5yXZUtwbBfYZS%2FIP32xmlooMRq10mjQIsDg3oHfZ5nWZXgPy&X-Amz-Signature=3f0839272ffb8b8f0c138666e54c42bd291eaf30c5346bc025803f348dcbaf23&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

