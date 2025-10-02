---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664562JJEO%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T190039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAgp79sQIHKp9T8El4BnVQh4PxfK0OimXCbplFdBD79nAiEAkJmjVHjI30sLuP5NepCdT83BksXbIK9hg8042NucTgMq%2FwMINBAAGgw2Mzc0MjMxODM4MDUiDIAjZyDZh6aN7sUMlSrcAwA0NZaNpvLbiKzqGA%2FAL6V%2BMhq8Ngpvx97v2QgqpJA15zlW2XaPcV8qXW6STlAuqbwgrD0T4nzuAoBpI49pZko7xA1Vj9cAI2YBKoT6CHrB6f7XsbO05%2FatPqEhKxPPXkmIrdE1VVHuLsc%2BI0iePh%2Fig078VmAv%2Fz0qwTyDJX0oVhUe9Ax3uWMBL0FojgM57SA8%2F5o2SlcIMULR%2BE2vkklRg1GpWj7F40rbe73W%2F59fFZvI8X6QgixOWM1nCnXfm%2FRxl6dpoNQ1oDG5qoBuXVE1hmTP87sV9lasZ%2B%2B620q12gwMqa6jyXLxejNVajjSbLBvSw9X1uwijZxx3bG63IYaoJDWFaa4uYtbDN1e3QS3JuAZ9kOkZlVTL2yD1G5UIHn6hL8%2B9QRvCLxEdlz2DMIKup%2BqmDUPox8rcar4yCo9UkYw7USRSfzVL0TDsonsHtcbg2Pmx4DFiEnLkEBCMbjB%2B5FurwP9lIPu9CIzg95ThlFvzzdkvjNM5VrwAnpKH0%2FSNntzGrmX4ffa2jP4%2FPVsBYJR%2FtFQq4Hgw1QZW%2B3h8dL4%2Bbh%2B8WMoA5Hwm6Bea9SOTt%2Bv8TcIR26%2FvjJXGQa0gMf15xjtuvG7xyxKJhQ0RNEQjCnNQWw%2FkXMTMLaT%2B8YGOqUB%2FTDoZ6xltUaal%2BZB1DanxY1rvnIpwSwdy57VnDte%2FAlKYheVUGnQIZwujMeraV706dXHA0hIGbQsbWFrYHUQxlkcgRGbgmVZu%2BpxcVy89wAZBxlF9zFQL6dzFg9CACnLWzzx9Gz7yGBzbei3mzxCElYgXlX881o4NXo0Q15JLmKUovIer2DRRdzTRfLg2c2BBlss9b%2FRoKzsIupjpoPGFkH3qEO9&X-Amz-Signature=08ce1c5164cb3e6e48c8bde7f2730b7c94477d601349cff017ce87e1f60b8d01&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

