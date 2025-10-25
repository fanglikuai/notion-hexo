---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R2IF6SEO%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T130059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD8psOubAN5pc1s6dN3E2G%2Bqu9mhuuuVV5%2FPTWPHgDwbgIgDH04a%2Bu4xy7byRij7c6tuWBJ6fWGJE0v87%2FXjgZ22EIq%2FwMIdBAAGgw2Mzc0MjMxODM4MDUiDCsvoilSbS3YoG%2B6circA3S56Z%2Bj8n%2F1FS1%2B%2BV4dX83oUMmew9hczxcVkju6ue1%2FuB6DUPZEQ%2BsYpEfkNIy7pKz46zFvQ3awlA9aQqZ7XuSaif%2FGBlnhbbbyBkgq2aseG2GOKD5dgC9kxEjhT14L5K0BmP8R8qVZ70rVhzFcD3EpXLSZGnKXsBSkdQ0E5SN8y5jk8TBF6Pa1S36xiBu2JjNgOe%2FIGKfVlo4zTsrTlo6nUI9eWHIFOUBqHhCm05Ss7ZgaBqNPCFw%2BxRMEW6cl3NG5L2n0ewACPyaKbjV80xKjc%2FGU3iVr%2FSaEBk%2FapUFjsFBJJYoJaFHQxM2E4aZftYkVJTrA2SZjIV4yfPUWxd8Y64emf%2Fw5u88Ksaupbnkq%2F1rdvNQpMaM1hjjzpcyBahjnJJrhS27IvQBFby8J2hf9Hgax9NsiOYFHzNNM42qKs6KfO3yYVxgpcYFfOPZ1Pne3uKTHGg28e6sTJIVV%2BpYNMs2IzHn2jNlyB%2BzFRGMK%2BWVi1hNM6GCw%2B2WPWpGPsr1ATHU%2BoxqPixBtySkMEgKgUhxcHb%2BTUVJULUn27gLuQ%2FAT56v5glzIHrKd5gFOEJuM10LyO4B9QkhVH4%2BUzt9POcpiqWgQpSMG7NuzQLusTIxgNEOi65M51%2FdtMILY8scGOqUB21Rd4qwLxUwCQvbK4KU%2B%2BelvLi8r3P4Qe%2BeJJCStunpb%2F0wl9nc3pA%2B8tuSHME%2FdcX%2FyWi1FgJze50A0sXwrUSDeiVSBFUX01dvcXi6%2Fr7yW48vvTb%2F0QWV7GDJquqX%2Fz6PnnWp3oUs4SFwjMEak5Avxwd6KT9GkEeIgO9tUhohhMi5OY%2F8gbH9jaZRB32lDG4He7vJ8SOozxHoqfF8R43mpEo3h&X-Amz-Signature=ad80cdf420dcf7166c986dbb9cc05ca1b91fb39e1e68dc5c0b3b1da15e5d5316&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

