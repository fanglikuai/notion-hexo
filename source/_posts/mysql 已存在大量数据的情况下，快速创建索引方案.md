---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XY4US3FP%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T190120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJHMEUCIE6NOnyVYmP5dTOe54z2P%2FRajl%2B3v4pvxM9V37a5mfZLAiEA6xOIlPHdBpp7Swl77NmQIhMn%2FkdyAMB92pYnvMPEfwoqiAQIrP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKIaag4lJF7IXY%2Ba4ircA4tc0Y7ABWY93oBaJqNbM28Vhh5E%2BE%2FQCaR6AXKxW9sBFa5hx%2FUQE6UENThN8fJnt7Z5fNV4NGE88%2FgPlx5aD3L%2B8dv5ccXfLui3QMUWGYZDQXdKCYJX0CzKdcGekJXufP4Xds3Ug4S4bZ0OAynx35Cd53HAVySl7mnKbar63p6dzsNt%2FdaD2dwZ8ZqNL3dxSgyuf%2FdrAxH0bgJgsMwD6wWYeSe2NrQlq0QQKwfMhWEhmAZtFb9ycJoN7UCvvNFMQciJl1EJgjidowE1ECE24UkJ2%2Fb1FerMU%2F5NlaxGvXKjUQasTt%2BLOKTYDAzUqY4GYbUEUtjaISmqD%2Fv0vVQyvMPiIKIRXvIRp1jzj55LuIw8roRs0qFSbMt%2BFBly64B49IPtaNHrVt%2BCvCGnMH%2FhgWZF7kPpNkVAIJtNXm%2F40W1c%2FVqp%2FL13pmna8v8H4o%2BGaALe5kwsPL%2BtcLP58nLFE%2F487zXEFchbXlChXBHsBkJkNYkaFKr9d%2B7hxz9xc6qTyjHfjyL8cWMVY2Duh8ss62Mq4vO%2FHeuNqbyKtTPJtWO%2BUFirD8feWLUdD7nUhU9Lnogr5%2BHZvmVZcD6ZBqJLQhf180wYB7CuLj%2FRpmSu7KwlUqS1R4BPjcOoxO7RMOXClccGOqUBwiWAIy%2FEO3I2%2BTKc9ePaWFqxCLevTiilFbps9yNjq1YX5FUZeMiOW7CajojDSxDqQ0ryaArcdgIDLIYV7mJIqlSifAnJwzzhzCCQi4jmeNChh1e3y%2F8fVWmtOACTawSGY%2Bvltsr3Xw6R2emHu9m2%2F9POhZOfTa0gQHJ8OO5vdIvV%2Fj422uCOKpztTJCUJ6lVRZeJevuc9a4%2B7%2FbB8wpfze20gbOQ&X-Amz-Signature=a55e166bf26e4e4718e0ca9c611dc6a1237e367e401c26cd449e536db03d1c65&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

