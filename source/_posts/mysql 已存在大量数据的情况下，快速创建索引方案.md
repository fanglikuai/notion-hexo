---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T4UUP72M%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T070050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC73I9fZX2sheGlrPtPMCUlgMo6%2BYQ%2BW8qXKFjlGFMVjQIhAJpzG2zeIeVnktlTLjvm833cgDX2MPoJBK6PfjNihdVOKv8DCD4QABoMNjM3NDIzMTgzODA1IgwQLmCbXtnRpJfDXMQq3AM4%2B4V77jgTtnQvQZZbnuVC1HdA9lmMBzyZFKy1fhZitb4zbclsCc6uNyFk6X51cHF98tyEzTfhqEsBNsl2GZdZ5DT22HpmS5bjrCL8nAZFiqHkwtgxqdXv8c7ic2rcWWfrlwdoyS%2BhVHbgAGdosJ29wgxkDb4rslgWlkNtb61C6HPnIZlIH9dPNI7IIjNcfNHGDnteCln6qsNLgKkZOhpuLurqY6sqFYspo%2Fm5ajljn%2BJVSIq7nVKQQ7tebPQjLx4cl6IY4%2FrJ2lOSLnGfwdDHeyxHoQdaLtjvv537j4RXPfDpQop5NwrsPkw6Hfu3NOSSa2o9PAJw72IAxAFy8sFsREJ49tWBpmaXEQO3Tu1Z6Heewfu2%2F2ReP8KY62cgVr4pDkGjiRHD5R9OsjtSDMMLMn%2FoJgK5EjAl4SqMxv8ynQOWwxJu5wd8rI0zm9p9RC1DUNpU6FIQHbC44TKZ%2BcP9lKMXgMyzPk1Gy%2FGUkgwglLSY7z6YKKeXYFHMZKl1uoUcwIXPRNOucF7lcDSl0uVk2jZIVdrip6EQLBUpQxGTbND8rqN2XPRe9gyqUuHRKCi00QclKvrMUgLJcWfgPSJ8DJeljiPYOljgnx7WaoCltApB3U82K2RV8jnQGTDXq%2F3GBjqkAeTVtQSX7Rs0vKQzfDfWRwhYUCTN4suP4zzDEfijVxgbEcK6bQ9JrKXHEczVEtn2%2BJoeScw%2BCjl9619gxL8%2BpmS70VBs4aDF9sx0Hpq91kv0K4uPIgwy6FjG%2BmI1wEoyiK7MMxGo4oyitl7xLLZOVzfg%2BBLvaO4Fr8mJDMv7P963rlxKjh%2B5y11tRf1fVYqiBxHnag5j57onGBqRKkEty0KsBk%2Fg&X-Amz-Signature=c3a3b622b4496406d3d5ef5379e630681fc3b16f776e28c596d352f9755b09ad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

