---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466URDY7CS5%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T150054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFeffvxpK2QKNFZtiR%2B2mgeV6hP3zHUg8uoi%2B1bSFBnIAiA4tMange1vnqkIpzG9jrTh2t%2Ffh%2F2gU9ljii1KRWi%2FKCr%2FAwgwEAAaDDYzNzQyMzE4MzgwNSIMN6AIR2ZNDdNN%2FO1jKtwDVOOiM5h540IAoAkYN1vCC6uueU8ENAAkW%2FnEBK0KmcFbnCSNHVelxhEzCK8IdzZukf3HPQykHx6jWNa8PJj1ywD5%2FvNXHbL1fOJLjIpBnOEO6fdBohX%2BNBLOIiler0K7wd%2Fwv4aGoWFb9D7X9uqtpQr3NoHpr00GGCpo%2FNKOKupVm3%2BvGGCcz4iki018VJMrL97ZtxHR9z73cF9YF3IO5RC6fJypguGqNxgeqHRzge6BClRf5vrTmePgAJHCc1ZsrH6wQ3TBrZo8rN2NPVc92ZJDr1SLPEXkExt%2B7ucLkEC4yBgV2CG2jN5W3dMgV1%2Be6I7h2UoF%2BBJG7FXGJXNWGAxfVBf%2B%2FwzfX5nn%2B6eaHtFBjF83Va3SFjZp2G0lB7peabtnG00mMBWvMyCpsFaGDId7QMXPKOH1vK4wDuG1NQBH1Mr0GM0Xqwt5wsOwZgT6a0avHX1ZhIGzwGZyIIyk7m06zppn297prmpJNExSC994fdJF89YXwfL8wp2UldwGlFeWC%2BIls1I4zRw0yJch%2FdbLFo%2B5SEayQ5Yc3ArEJ03iaLukVHDT6ACYXyro1X8dylz%2FAcyzEOcf6mDaP7cvOvVjeEsMDyNr5xTUE1L1KKw7yeBf8Q9A20cKEscwg8DFxgY6pgGIVL%2F8LM1VBX3ccgonCIfLhRUzPO3WjEoelNDoNm6%2B3Faeov3TN04EaxfMQXG2jTZ03UR1vRYQj6zHhsJSCTWssoz4FdeOjMQNUYQWBAEgeB5zIAcK%2BX20kuJIBCRpkG6313V9RU6QTfiYe5hFKbb%2FaSWig5uejGOnCjVGbZxhgG4%2FQilp%2FFd8hPsXgVM20ewMS%2FG1yk65TRZF8wOC4JA3Pro%2BVCd5&X-Amz-Signature=1567e689205e94c6777267f4ab1786996a26796ba66193dc8a269c33516d96e1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

