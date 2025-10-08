---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RXB6GFXB%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T110042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJIMEYCIQCEiEqY2RFcJaHYrWWWwQvmo0piZWLvwCA3xk6vPZKj%2BwIhAPR6hMwXz8LNkG%2Fw8jQ7ImvYecsPAK%2B4pF9w9dGVjAIsKogECLv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw%2FGlcfjI3znY2DDecq3APgI6mnkNgWC8X46VH%2BEJjATqGLYB4TzXHvbKHXSuImZY4k%2BGQnxU918v2XJYlh1fS8sNJMLFy1CdP%2FZQW3KqKsutRivQoaEhGYuLZhQyNY6wsGlgox3z13X8XWMpnAl7tTmXBeXbGY25lMvHLetibsk4u7kN8e54Fg9wJKm23LQn9qyEFZyYoOtAC3Zh72praMIXxQ%2BEXhBMvQNcNsikcFGxMLZqlw9D6kz%2FFKxzVaGrcTdQJemO2GSJi1fQ9HyHEarqgLoVQIXNr4i4eq7QFG%2F04gld3PvlmRu53fWXLRcZWCnZXCP1ShpLNY4lnGUR57lhsQoqinT7UaoGBsBqoYeObJSgZPHmh18rE9fQ5ZFNzvwS8xMrC2qJ7QSbzJY%2FvACDMaSE8SY3YXKJmXMwmZquEvrjrDIG6am27OTv1GNke%2FtEMTZto6K0%2BCFpS1eoKqRLuceEkljNvsksGynaXN6A9S6nXGtWZpol60glWjiBopv0pjx7bWm%2BqXu8T2qrPgJfuawwBMv8Mo8212rHgsxYpnp1VG0DFm1gP1luadjIei7Szf57dWqCv8ItOrUmbuubZmFk7mh3k7%2FFmOF%2BNcoK3IMHFp4VycwbOO3%2FsMrOwUSa6at9liyMmfNDCx6pjHBjqkAcZul7uUdrOQbLENQ7txXEJlxgTBKI%2FgWr55npZYDc5F4j%2FO6d5S%2BnUNpzb0Xq8xUGCAXD%2FMTjpjcZ83fxU1S96NtXBF7wQU2%2BtjuyuQsuDg3bda%2BKFOBPK%2F5goNzoUXDI6b8QgkwdL8gEw7kKigYm30fwA54d%2Fg%2BIXQ4o8m88eIpOS3rlI4B2H6lsyyxX%2BvPK5dsnHBNKVV2CpDAh4Wzq9jSkNc&X-Amz-Signature=eca4658155529a4ff32771c8e175c873c424b2041a7064f3353bf552ab30af15&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

