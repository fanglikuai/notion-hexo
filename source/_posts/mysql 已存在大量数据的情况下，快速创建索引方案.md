---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RTNILZVK%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T000049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJGMEQCIG%2F%2FXWflu11w6A8gwyPfY%2FK0sFVShBKLqvCX0B1cObm4AiAFckh%2FYi1o6agtAkAoYd2kGyZFgn4tMxV%2BWwJqMwsMXyqIBAiw%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMeNt17MILK5JAlKtdKtwDI%2BlaXAphEcKFlTG%2BJACitQqJ9rjbOic8HPg8j%2BiNAxGKwFHZ0RpoZDKUjj0yllX8VpfQ6ggclbNT8il3y8BAb%2FHbut8TR5iWZa%2F4GDvC%2FQ9lw0P%2BJLET0pPeKFCYcA5fDDjB6wzykdqmkZPY3Ae4M%2F3mpRyVMSZ5yvQcd4DiDMZtz1NGhlgSe0zUiyPU9TUhbW1X%2BDq8jUJrAOEiI8u5kVrWIFQQ6oFQv4ruB5tkmfauhN6K7Q16k81JfazEsdcXmI813ahzqXXLVXGsIrByypzgkOcUsXkxLa6O0uIuXzw8Onou93UtVkgS0lrjHVeRLiKDixTuQPlU7HboeK5SgMe%2FURNUZp3bU0uD7POV0Hs9xJhZJVfEY53gGOG2G0aEEEWNcG592qX6n%2BWzsFNp2%2B5uzQ2nyjk1eM%2FeNMg0FLYEg7CQgumBiaRvCmGlrdDgxUhw7OcPeGHc5tlNoEp1mfXmFuvQvuIt5jBY%2B85N4uYckBXiIVKDyNA2%2F%2BP2eecZtCajKuq0HTGgLaNBnXUAukvf3KdKalKHJevqJfbE7X2sOENaC3Gfd8ouZ7e7Ajt8AP%2FHgxDXE%2BeJ6hmL9hw3UgRVFnHHRaD3FsP2xgF6PObewbnzRJoKg6yBStMwkZvLxwY6pgEAmZLpLj3V4hVNsDSuxnTE3JB6q8s6hL5hPMRB1lnk1ELEdIzFSeijTrmgKsplYVlMA5W%2BRw%2FXNzYkVJXRTP5ICL3E5l%2B24zpoYZtq%2B4WQGgWhClbrTeQDNl76xBfWQknI0gvUdhJrgbySuEQV32Ik5ASYSZHqUEDdVf5DI2ZyeHeuON%2BPU4F5w0gSP75KGYz4f9iEUK%2BxjHDXgnOztGCHWPjtal1D&X-Amz-Signature=1d270845077ad36fd1956d6a7ebe2adec42adf7d5f5239bbd953f98daab15538&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

