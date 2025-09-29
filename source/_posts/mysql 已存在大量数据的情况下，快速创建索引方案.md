---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VCBNXBMC%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T210046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCIQCUCe4mTsRGAOY%2F2LfgSaja73s1tZlgZdQJ8f8gxCh0WgIgBEQOc7Lh0Z3uUyrygONtqMGjHpNdFx5dJx%2BLA4qQIYQqiAQI2f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJeGDaSQEfR%2By8Lb5yrcA5yusF8nnw0Rp4h4ZXDkp0uJVsYksPyNeIjeX%2FEEvyQv1lKbL3MO%2FkhYrTHR6YFFuAShV1LB0FFmDq5IloGt2E6gfa8Dh95leWjpVGa8gKiMVg8P0ytXh1WI22MS32xWtiM%2Btup1d7wsdh3buGXVaNcK1kY4q02nsy9cgShqKMfekRz5uItYjfF9gxdOrFTsKZ0ldouW0vK3WRQTaOQ1OIJkhwTWPvJuDvwrOhMhWXn5UOsjqKBIym22Yb3n8XsZBo8OLFtdEPl8ONkECRapTCV%2BkFI0eyu1NlGdToUxHsEzS0%2FDy0%2BV7DmnphMKGJqQVk8IzggDYqJG0FcXLmDp5P53UpW7FK1dFSC5BsLcoyNpgbNs6TJu61YxkPP0A81R%2BTNP3jDUKgMxrEsLAgtPDCA9CR1Hxpsq0OLNnhZctCQkDWy6LIMpp7ZqnuBaZqKFm%2BYabluu1miy7qfqFVpKj%2Fvvm2D%2FEpDyDGSKutOI5M%2FbjrWCq8MD8JkPn%2FCifk2cnFM0SDpfxrrEEuoCPgFj043XOKYujcI9tg3eA3UFJFjbX9RBwIA11uWtPojgyDEZThGazp5jgv9uG68uEtDoCrnsfTCyFjoXLhBu4SPR0ePcK7EszgwxkQ5FfMGfMPzU6sYGOqUBwHkWzAtKiRp2rGhwgZaEYvZYnLe9fLvCuRNpr3PU%2FqNJc3JidGLfdFAC1MCm3NWAiSe8TjkwunxtJaeVCFN4wxQvzEpD2hAvQzWdOhXj0o%2BeDl9iIv9IR%2B4uiXGvtLoGZYP0Wi14zi3UzWhWk2NR%2Bk0ieTYhslWWmwKoqbgzxEL31Zp%2FqYeh2jdv%2B7M%2BIOJmGkGFvv36C37m43tEY7bEkCsDzl2L&X-Amz-Signature=6b1399b7fbb443cf84c9dfeefb86d8e50d4516a13da2c0afaa5208a9be70ce57&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

