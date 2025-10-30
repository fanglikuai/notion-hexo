---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSBYPRZN%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJIMEYCIQCUEeCvJPxhY4bNVtgZM03FMI8LyOR4h2gvxF6Kx102kgIhAJjutMYMK1mSlmbiTvZBb6U4eOC0Ecs%2F8xSosoikcTwRKogECOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxU5yu5wwM7fUcEKhwq3ANseY0XsJim6pi5IK%2FNijVutH1s0UhoS7aMc%2BfLt58iAp9ErMEp4NqpTs5ecUYSsoghCCvxw2QvA8iUeRDkvXSSQ3RBcgxsshMatNI2OYbN4q4z8%2BTOBQaYY7yfyaaUxKnHrEL7tVKdLLIjH1d%2BdMHHLPSWfRr1HYoRkH7aJjI1au8Ak7nPAIKBzbf0t0vtbOwVsIdyVrkNqqisoiIvKXPm0QAe%2B1WCC1DxIsbmS2jT22p%2F5gOAkJQr4AFQZ8P2CVcNmw8UCyKzkvZwie2SJMwT8xbM0wmmtZraoUjqlyn0XILX4nOL0lehV8oFh10xBga2JS0qTS7FuljqszH1BdmV9SFbAVET42tDG5Z2rfVjWW8EZVlYOf9ESovJKdyiR8oKvdzUGMfBY%2F%2BJuXZdiuXETemXBloQsGmNszOkeuCxm7zwj3VHtulA97lgd%2B9fZpEASvw68cUsQW5T5DIh05HwKLI4Ln3n4hnYTtXqjzZZKLAd8CFy7GcchT1HalS4aNZ0BULG2dstgBv5T7XHG2sG45tj7Ai1UAIlHFb3SGTydscaziZ%2FdvFt3oVrU%2FCfS0g8F1tQGtCS%2FlsUtK3x5MgE6SBDoBQBRkbrJXuvIhQjf6hzkzFy3dNISgyj2DCXgYvIBjqkAaBB8AziR52yeYvXjKXd3yp8ECbcPMHkokZdqHztuLabPFHgn0E6n4HtckgRh0e1YihvnTmI8rCObovs3KWTxpGG%2Bg%2FYLZMbD5xcrjVyRYZi2Ww1gQXotu1jacykhwwtdvwVMImdOlIiEgo80JNj3R9ufQCEdsD73hR5YIIkY%2FXY3ojuyD8uAgNmPqDLEhW1VpAPX6duS9I1b8T3u%2Bfn6E6IPYlM&X-Amz-Signature=5e2850d4642807e280e67b91dd403ca0a222c33cf2689599efcc3bd6777f7ab4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

