---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662IP7Y4JA%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T170040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCoDTS408ivHuZ07LKNPy5lYbTP6qCykVUU5QjDF82GLQIhAPW3VZHJauu0LYrMIdiPMSt41%2Be0CWvKp4CcfFhJw2TeKv8DCHoQABoMNjM3NDIzMTgzODA1Igwvpx%2FzOjf4V9pkL%2Fwq3APZzYYc3iE0d%2FBZTLNSxlaSyzyNH%2BhOWZQ3WuW8mMKH%2BXEGJLwyOBsQU6ziJcIy1JY5r1hLZWypjDYUJJ%2BPYTWrEoO3pqgt1qOaeooQqoekMh84LoGpEvAcmUNSI2%2B0GPLPoUL2sGnisgHXVF4ZEffYFUBbbcXGC9BmPWw5mh377vCbzzpUyYjglC99QDZgtWifezvmcW%2BoaVZAED2EYlk%2BJRsxxtgspO9nYG2a16HXUheZA4ICmwKVE3G6LXhimjji1xG%2FCHKGF2ErbUoNYoSwYlTUJ48SGswczqB%2BjkX5wNz0QDn5Q0qMt2gEG%2FaMsgyaauiucsNo3t1G%2B4iadRTsVVdNAsgVjkonYj2UwB9CNBcettlS2S2atmRQ1ysYa6X1ydpxjIIeXYeHLpmwf%2FMP1JT14%2BFzw3oEr8ZsdNdY1g5YUTGhBJhlqcXhS2qDCKbJAICZ55n%2F3%2FF8eL8GHxwYUZlUAIIMqOZXuGH7aPLqBvNLDqzZxdsb4HPzjFXhyOhF36nojiDBDUPmXSsYe2Rpk%2Bv%2BoQHntID8rQn7dSQm3AQaKjpKsPlgnFSZC6uGugMXw4iO20P36dh38ZQdrY9%2BBIJFyqOn9ztQbJawKZR9MktKsR9LlF43TggrZDDG36jIBjqkASMBXgUWuKsjnvwtf8ORh6yL0X50xoGBBko8O%2Fmy%2BSK7qDEvQtqepn8jelmRrTrmPJ1IBbunh0xaZGSi7u6mrxIp0kHf8%2F4%2BcR6Tck41DFy37mSbfcImWt5JzLwyi5EQp1FT9ucVv4SpywzC7PIDojm2%2Bm6IoVzMAwtoDK0KEANoBuRHeqAoVCWlBBLjkuS%2FARoiEt6QhOIezz2eq%2BOUIETsdo9T&X-Amz-Signature=457c6feb80a1646bce49df818b914a3d5d971048079de32caa680aceb42d1d7a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

