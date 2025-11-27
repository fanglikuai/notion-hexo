---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCXX7T6X%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T180039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHqi3JH74FPJtbV5yvwDnrdy5hjvHqEGetkf%2FIRqmEC8AiEA2JkXHu03COhEaZXHCdWft4q1nfR20PqzTZYb9d8l8CsqiAQIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGA5cPD96MNAzhdfYSrcA%2B9GQryNsCvKFVKON8h69AaoLfg3K7lkJNBRkEabp8NG6l4Yep6TeyCN63rcTGXG0sEUcIGTc48%2FrYYVbFGTbyTiA8Pygay1J%2FhBTibEFpWTYJjChh66O6Ez9P%2BuvD%2FczxgglwFieT8M2LGgNrPFr51LYJxH6IT9W43d01fojDAnE9oXR0md%2FC%2B%2FLm4VdN0ex9ksw1QkxMSdhLJO1NXKJZv7WismqgJLPC1%2B25jN6IEIsWPjOXetx1vFKUumlsu6h4VhWdLCOhYUaQQA61WNfILwVQqGQ778kw%2F%2FNqvKz7TUDU9oh3WWuZ%2FZXF%2BgUrVYBKkwtwguo5SpXgwZqhDVKYzZK7zcmyP3%2FIYCHh6S5Pm%2F%2F%2BbABnGxnvCeu3fqfWcvsJlps6CiwuNicQPk4jDdN%2FYQkz4A%2BigGX175gP6CvWVeZRVQAYLxC4cN0LanMQvd8Ba6piOoM6StRCApdnD8DJGOs5%2FNq21flYz2JYqsQV8ujoXj1vKWcF0aeYLA13b0tU96SZvJ6jXX%2BevqSixifsuab5vMIMkacdsSSPwhdnXlXoXAMBDAWpxl4EuSEkriIGLMVme3sgC3DS9rOAZzgdsxFIR5DlEVkuc8S4NGX9p2L5O4VtlMBet3aatRMI2iockGOqUBwbZxWll8RoutxXMK1E%2FEF0HlTOuCwBQYSD4T2OVEKz5sqNcHDMOUlbhPYnwYERUyL0WNfdSC%2FvrXItEmUwKyHnB7IJ9YIZJ%2FOtc2c%2FMVgpdyjR7P1uui5XYmTF1Y9Oi7pBNGRzhPOKyfNMb6x2bT%2FAja2Pe27QQ2VXzc6wXUvCFeUiwViCGVy%2FbCoLAzFbJf36y4cqzFjb3%2FGidfV1ywJon5oGfO&X-Amz-Signature=e9f83314fc1243d6370683e7e74ead17e0a272ce877558719c0c2c1d2b726e74&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

