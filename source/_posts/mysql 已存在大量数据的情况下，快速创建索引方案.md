---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466URP2V7W4%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T010047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC28MriqqGkpa6fIoHqPFjbylvHSqFNVx9%2BBEXjXonGYAiATE7tK0qhzdi0jePP1IFovxvIC3aKLVRkZScazz%2BmJfCqIBAiZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdsEP7a%2FutS64TAF3KtwDvdBgycfqwaexCLdhJDLhnJmlfKEkWJquMnals43dhukhtZDbjkmNvi4dGjFkzW4uRzPvF7SoEjZy%2F%2Bin%2FaN0glnaZmW5lBU2j1GLMLmfcbLkH1CWu5z%2F65HdkiMXUCZtqJk3b2rjq%2Ftw%2FmrrZX0Ni3jwYIpjRr%2B4x4uPY45WaFcvFhUexRYYZA6heQN1OBN6T8G35oL%2FkCg7lhGgCjhnq84Ln1phhNZyRjiycx3kqeOJevpS4ZLUxirQpIBsPnf9MTBBZC3oK23%2BpvcF1ov43eSEZI0LFk0fWU%2B5o8BHRmkMtcaLMBTM48EzCpqDr1ywVas0fpN099V61dtXMx4ZVh6ZlRq2e7ckD7iWvtS%2FdEADcsofHjAOXNMm8t3j2Y%2FoizKj1fNWqV2c7yNOMlJasGs3SeuKQpPZk9PUNOjllakckNkLF0imIYizHGag8qfw86yz4FpqO6dAgc2%2FfC7D1IwPIcrhWSgLTn%2FdIlNU0mhNUChtvA%2BcxvjfOOmMerUsQAGsFmHuzb9uduaMDs9h0%2BZN5EwTjj%2FiDuO1NU3EfKDre1TOpzPYtSydz8O%2B3u3CcJ3maXNZFMhl%2FXPBItsozbhhTa%2Bc0cARrTqymtK8Uwk5GK99i9IxHXFeqiUws%2Ff6xwY6pgFawQYAksOQEdIXXgkGmo%2B36Y52pR9qs7u90BZqMK6KK58l%2B9OAsZ2un1hDfNn6vJIFml8emTZOJrTvRaZGFmvOWgllakurYyIL1XfwH2EGmcvT2KZ0%2F5Vt8KmYlVDQbLb6f%2B%2BJ1cy89AY%2FV5g6uwlWXXPauvdOMq4v%2F1FG4DXJXIJEJ2mnv5klswayltBdq0XQdSI5tPGM7VufvFgfw8YvycgBoq73&X-Amz-Signature=a9331c7d325ab1a117abe88d2f31b5eb3a655865b533c8bc183c0e54b53ca859&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

