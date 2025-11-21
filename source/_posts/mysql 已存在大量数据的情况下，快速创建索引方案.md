---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U3FWRJPS%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEsaCXVzLXdlc3QtMiJIMEYCIQDTCmJIovHzgyUnVEFbmbWvYhLU3bEm08I9CA4Y42TeJwIhALyXIXMsGX5fJbWMhmhxBFYTwuWMAmiKA5CbfB26ivRwKv8DCBQQABoMNjM3NDIzMTgzODA1IgzO2N72CxtojCbP4dQq3AOHw7IDmDksr8%2FPfVVmp383hmdxMPK80DTT4RRRTI6jbfZSeKcCoWIOZsI7ZbKJI2tAZw%2FsULFNvg%2Fi9imkY1BS5XXCQT7J1c%2BBIYbozgLfJegu5OJ3Cpxw3O8P046C0dMtcyaV0k8XZOw07Wo%2F0Oq1lI6DgxVS2DIy6ALDywPG49uUqSKCECnScvc%2BEzJqPqJ7szQol9hY1OqLTGPNIXtdQUUEgAU%2FWzQ%2BhoLMrRjEw1YzfS0TqS9LPPKQub01oKO4%2Bb5ioEv6QPkjh02%2BUpCaEJuNb1ZjuO42InE1yns04V4xlR88bX35g6fZqAVUSsLFcHrjqDVlptXEy1NszPDLgHEZy8rMio%2B7Wygixq07Y5JvZsFDjRR%2FyWxMer4xNQBXtCjGDnWVvLK6WgkvEUtOkNHSLMNVXvZeGybV2K%2B2bB32R29yeucrX4LgH6ArjSb710EcjV8p0YqBHbOmkYK7aHdQWJ9hnv%2F0K5hO5M2OMR0QIvuXAhrlvgDRf8ktHHKOW8u4WMXoaLhd4Kr1EVeZTrgxPP4BjXOr2Wh%2FeYAOvIEFcbjdFHg2XwGl1zP71Gw%2FD8Q2EaC0G1lC%2F7yO7Od5QZv%2BDnfpff%2FhnWsznfhTVtLyGO%2BWuJFnCX2gfjCr6YLJBjqkATDbY%2BcVN20ywosSQqglGamwnA0sDpd6xLG6r0vXULn1XDyUxQisvaI1PsmIR3K5hXXA1jaFQZz0Yfq651gngl7S4JJ5oj1I5CRvTQMa0b9tyuAkFnXQHAY%2FlfmXB%2BxotpssCtsLvDDi9GbQ1xyJxnDloy6ZjA9FuvGTUoGOg7zoZd5pJjR7bQD0RzLHNHaNh438G8%2BRqMwC%2BXf9RZueAUrfu6JP&X-Amz-Signature=83b99095a36095087644b82c973074d63eb17cc4402bca7471843e7512a49942&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

