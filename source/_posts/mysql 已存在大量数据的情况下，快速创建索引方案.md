---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ROD4GWCR%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T160041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAl2rZo%2FjstxRd%2F83UhB7Bk5g0VnaDQJtQuac37OINDoAiEAyLCJxXUSYj3Hi9uR8LtibyGNpkMWY%2FB3rPLLBTY7fq4qiAQIqf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAEDktmlAQ8zb%2FPxWSrcAw7882NXtBta74ZAhz7An%2BrHxIDtvg2AqEqLBJFDicnePLZIEjv4PIvVKdUfEfciSptN4kxOW4%2FeKwJpJ92GpkWam6rXySdFAieeTwwJRjMt2Hv%2FoiCZci0UUg1FZeTBmsWvfyRTZ0ANXDgdcoqSK1iBWzu166zwo8JkuQDomc9EhMf8jPHejHiyyvifWR4QqiIqhZ5iS0vmay6%2FIbKHMEJdOTSat%2BeBFqyEfXDXuvlUb0nkqwgQuxIhF4uQKQAyaYe3F7I04lu%2FyH%2FL3H9fyH2YX%2B2meBLIdsi2Ek9jYQ9nhvYru8ejPsU4%2BLbWlgo6TO3UO8sX2TFT2906qPNaSvJtdCqSRx%2BrRd88e8x8Cfcy8zHRh1BOzkfpLVQP6EwlqbRW2fKcKB%2F%2BDiHGNBSbJg%2BTE1g%2Bzs%2B9jP3zXr0BSnsbYNfDXIecoEraEEopcAo3%2BZaoiXs0jMJVfv2RshNV81HAyYF66nBu9VUCZIdX5BJl4506RlcDa%2FVL0KUBEhvrMZtg%2BCVUv979sCz3cvWzhZLKugxhSw3bT%2Bon0wXeZ%2Bm4SFHgmA4%2B0gfb3HKxiR%2FnB6NK3Q1VlHfyQAoD2pIXgpzU3UqH%2BzqNGJn06m0WNEGH4ro3zZyLU12TmEiSMJCDs8gGOqUBx5%2FNvV65KTqKvVLGgIeZjOYDLGZNBVYXCVrku3JGl5nmWrjz4MLIzG8I0kQUKB%2FQs0fBqejOp4kR%2Bpg23YFcylkoZP6yYMnITlfy5bWu5rrw3uzl1hH3DQx%2B3WdYZKbtAXncXdSQUGu8DmNa2Vk2bEyA7POKAImUWZXxb4ksG4CqVZIqSbocwZBCrpTwBe33zCVMSGTh84JFMJ6qhakbv%2BaiFE68&X-Amz-Signature=9f59ee56751cbeb12ef9259b73e3dca8ec37520bf08399c0d24c382a18481d6c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

