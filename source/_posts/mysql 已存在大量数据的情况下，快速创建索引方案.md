---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SKGM2XBM%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T010044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIC5KArnmxZV9k%2BvtS3Fl3I7x6Z6YaRHSGERiqFzE6kK8AiEA9oYPuR7KnRiyjUYd2HrjlkGc%2FVUpqtCGYBTNJomChqUqiAQI%2Bf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEeGLNgTbe6Zqi4CoSrcA6i%2FqaNmPWAuYtfBeKs6%2FT7bXjhEUdFNyLWNNlXa%2Br2crOIcIxuqjZUAOaHn1b15N5l3rmhzF2ZGLNlqwXeaJhZ5Czq5uE2ZI1EOlBRdHqXG08DZrbI8HPuiGoG2x%2B6lvOTc5lGOGqT%2BdXzqyCedOAgjll9PiOQD2TRfCQkNCCJMUhRa7UTKjLBdVm61HSEEJtdBaG68qlGSoZR2pwH84rQ%2FYEvHHsUGCD5k5CvNJBwAwk2VGmhNKUNvIC7rSYbs2vF0cNQ4LDtWwTKHJB%2FhrQTF8ZxAazWCScFCTOU3UrPsDeiIwv%2FmrObYT23DV9YW7%2BUNh%2BxiNiaPuMb9NeqZdzI9SEpA2tQO2aHw1nj3IqiTv4GBiQateKJfXzUDPhqWnImohXRWqbocc5BshpgYvckXeno1VGBGqgW9LKaibdcmSPHa%2BwF31DjJGuh2BT5MVZTXg2d7ygPDrcIW1NJZhqS%2FcIF1XD%2Fyke%2FXh21EgHLnmjVGXt7Q758Q3Nuk3BHpb37e29y3p1JFZYM2lUocqG7XWZtC5PtaEi90Y5yFWBHn3ULhCXu5bABwUE315NH3zCASsYJOjeOObToqiNYoOaO0ch93qen2V5mmP1KyBvQhsNcw8etjKZRwwmNxMPnxj8gGOqUBF22bz3KC%2FhJI6vJxq6EI7bvU1jkm6Yv1q6%2FAj1JXbq4sHMsFUQQ0GaPMoJdrZdtNQLHzdn34WgQU2p7hZ%2FxfUySGYSJZXZzXNIsp5gfexzLHaiU7kzU%2F%2Fr8veOazJu2ZWGEQbI4EzxghEYojipSlYfh7Tk%2B6qIupl9kzYAJK8asrQCxiuWx4QaVHLclxVI1fte7owi0RngZkilf3UqAtKfgjiCPw&X-Amz-Signature=b85aafb16e3c5bcaddd0b8c3aadc659eab0a3cda97a6103e855901386c379647&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

