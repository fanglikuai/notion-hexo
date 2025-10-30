---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EG2RERP%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T170045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCICvllAKGRtT%2F%2BUmWIpIgoohKfTKhH2M23WwgDrb1jcJRAiEA18Lo0y6As9u96cqpXEV7JTx9HOYab3vwqMQvrma0n6kqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCOnFN7IQ4Hu15XGuircA%2FRcw5wkqepkZFY4U6pyvobCd4ClLSv2Jhl3igQpNfXKMkCiq6yzrdKIy7J4sufYoznKmoRyMJj0dJ8jobAMoOR2m1sNAvyERVGbwGfm5FQqGI%2FxlLNxzc6FAvvgm7SJj5tiQDXjLIWkZu8qOHs6AkApW5X2hWrzmlBhzEl07DSMTVWXtd1EnF3cfvYfa9DTOQ8TLJ3JAQMaBBA43TWCu8DNgkfxf9eLaBa7joPKHzkvvWYRC8VW5pIvSiu6cyE%2F%2FGB%2BuozAFpps9gNhXIihnPouK1cgBZ4JxrkrE43En5AoliayQgjuudZA4Tp0NbFezFUgcgmnv7lhWOdx3bdcEqpgcHe5kyT%2F2ShNZkpWDpJHlxaPDtA9vgL1%2FL4fm%2F3e1y4ycoQmvnzrleaxPn7S8m6jwySbCvCI%2BnxHRYSyNcwN%2B4sHskj4R%2BrWqvJXfwAkfT5LpF4orlcKYUDyocIyDMZWfTb5kZ16zoJ41C9oCRmyUguDxqSV2Y%2FnCjzqQvxhzQ5F5SAbgCLjyUSRREf0nMqywi6KlLBGhVLZWSgOzcGz%2BL9YA9P6LeXzfMm9cuKw1950Ju8RoAGgNKKkRxs2FVVOhVdAeJBJeoac0%2FUFR%2B9ZL7Vn5wlRwmBxNQZVMI6djsgGOqUBBr1hUdW3RpttE9oytJhgN48D83UG%2Ff7QFziafkRzQ2a%2FE0x7HVMh5MjJVnhHRBt58QT6GqwmCoX%2FLRHxVQIADbkwXnwWyvp%2F7ze6qJ7EE6Vl%2BbyIuBKM8sSGLzgisghBy6YuJNZSfz3CMrnjk2DY8OmyE08xCxx4gClyAvenLF1epglfxGdejC2kBJ1uEmBvs9I%2BVAEjWyjcrkgqyIQqCBCRm7Cz&X-Amz-Signature=ecf34240912b2a033dfbbbfc619ed65254d218cd8586c4e89d5b3a33d81e4cd6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

