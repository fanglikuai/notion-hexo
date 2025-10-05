---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TIZ44FW2%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T220037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGIzuTagkvvnO7LUb%2FCYMeY8GGj1iFfB4TILY0PaA8RSAiAwhlBrlgv2gdGxMx2tUZdaQQeUgNCXukU9e1WoG36kuCr%2FAwh7EAAaDDYzNzQyMzE4MzgwNSIMufnFe9wLQVYV5WEHKtwDVTPb4OyjpFXeyYvo632d8dlgoeKmFSTzHjrO9PjNucBGEsvjxOzxN5wcfFc9UvJU0RfHToIG681Qbth0aE5I6%2FVg7DTl3%2F7uiGR8Fr0DkMNDSrSxK00zUp%2FzmAqehsz3%2Fnks5%2BOZfQ2c08vJH%2BZ00jjBB16wxspAN2HOx5tfeVWSG7I4DvnIl6BcUe%2FM4T1XsL5ZKY%2BCcfhDJbogvBuk40MuLmzv68Ypt7HncrY9ASGQo%2B4PjPSLJwrD6EBArJ9fSUUbd%2Bf7Qsyx3kDoYQ36oeTxFO34I3br9aHCtgbw%2BS%2Bk54txTEQavGN4Kp%2F9xc68G0Yj3CpDaAn%2BH0pS4hfR4ZGBUJ6H8YlrJBlLtVEdnEGolTVlwfAHdDukUdqCup%2FBdEWXurHjCn9%2BgB8%2BjAjKwVwVsXHSgU4ULxnH%2Fz2fbXBxRXTIeAcIWujhiyEzb2k%2F5cEoVXUUyQsMU7Q0q0CEoYCiIUEWvOxt%2BgtS8B%2BYwZ6aQxIYQshgwpH0vW%2FmJdbPg0RJZZBkUWTuwuungHs4ozg%2F41fqxYC0EGxZoxhrvqEKcsi%2BVeq%2F09DAXb0U%2FyBcU%2FafKw40heQZPmWYtwrrMq9Q%2Bc%2BadmaNhkqdy08AAx6j8yAsbPYu1KRInjcwseiKxwY6pgHq%2BSp0ncrzpmSExKrrpEDI8w2t%2BjBJvF%2BPfuG2BhgClb%2F%2F6wT74spKXBP1toXhb0BOVEJK0Fj4%2BNf29LH%2FaDhEh9FxNVyhAFUs2JldZpl35Km1fyz2NJk3Pw7LDQ6hCA%2FURwdr8fjzis8CMCTSZdbnlN3j2sdVVtKWgvv74VdgeovfLHc7pqoQUw11UMZ9wc99p%2BYvNvLvyBduq4D40tp5hR5z6lqB&X-Amz-Signature=0fe1b2257043f0d897c35e02e5a4c963378497e8e925ddaec2ad33cb09df2205&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

