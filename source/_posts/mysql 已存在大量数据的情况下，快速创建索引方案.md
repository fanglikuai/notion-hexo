---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YMHWRFFD%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJHMEUCICJe1lfU%2F2MCYSLLgF0mIwN%2Fn%2FaPKG0EYZwdDqPFTDaJAiEA5Pi4Sg37uCB1e1lRGnwWdtrboafq7pJcTwHANgwb4Zsq%2FwMIJhAAGgw2Mzc0MjMxODM4MDUiDI16lTsP25%2F5ukOe%2ByrcA2gOY2lJLqNQcnD7L2D%2FXMon522ZuXRZ03gOW9asWMOJlqLTTsex9XtU0uzdeZiiTqj9y2Nkltnvht14q2wPe7UYLzqFMqK5DSCHJwo%2FASZw%2Fp%2B4gVlAQ09bOkbmPM%2F7EQnO40Xkw8%2FmqCez6r4izY%2B7NAr%2FkcX3Y2o1w%2FdiVSll7TvX3Fk8nyGy3M4mYQ29wY3N1GhIQIfKWrw%2BRSPOVGdFbnlX2u6kXHV9BeNLVOJn0FHpls8ZpzI1zDTclbSAwiFICsvo%2FCX0G0SCEWY1KNAs05KGjvXNxilDDhENAQXrKuaik8g722%2B4LZLUUrjLZ%2B%2BHjjzaS5TO9uEIvyuJNUapy1Hyx60F%2BM9oCEtoFKOZMMG8SA9vjhmGlqbXPJpdeTjFsdgi5gTbf8jcIeJVG8tAb2AMk7XKH0APON0u7VBMgenNzls33WwvAOG5JjDgcNh5YUavnnd4ZJIcfkNbxggpGLKwKhr5uiIFWvAaG4Gyb%2FaQyaXiQI2hfUEvfB5eY1yHAaAIKhYptsfrs8DJ9vnmg2xpyeW%2FSU2l%2BMJKZEWabT9AbfQ7U9ldpH2FG9pZlt%2BLtKhyhtUUKUzo7PqpGLGQnF%2FqTEWkSegvQfW%2FyPlU31g9DwQP2Igv1jQHMKHIzsgGOqUBGFxhHAKh3We7tSB9YSYhGuF2Ghtll59Z1%2BjNhgtc%2FGcp4j29JQneIxD3Y4B4HQ8jQ4NYnjbuNdYsXXLXXVSI9z%2B2kLgNARMQzVaxNNfWBJikyGbaG%2FESbURYmdkZZU26%2BVBTDr9zSPhpr6EaLZ8lLuQNmdh4TojlS2ff8gm%2B6XA6KVSiT8s1VObmRGV4kWR3uhXK3FXa56pG%2BkwTa5OPr%2F82hEnz&X-Amz-Signature=4c3d6964b85a07c5de6cada3a3e5d1c292c5dd97a0c178d0edcedee7a493b111&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

