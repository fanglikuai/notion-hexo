---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R6V2MB4X%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T020043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJIMEYCIQD74PDqxyw1Wqcu%2BJS6TJZxXOCdxo6PdJjtosicj6KnfwIhAJpv6Y9IzQei1tZu%2BHKjnf74wJQ0Rt%2FT1BBCxZohQTwKKv8DCCMQABoMNjM3NDIzMTgzODA1IgwwAi6dUdnc%2BShoKKkq3AP1lM7OddZGKI15M6yNJvuuMnOEZTpz7Q5VB%2BEG96yqAYNWtIQdXmsg8og8QuGwIEI5fR0Im0Cczrjx0%2BN0aVoNrTXPuVYU6rnUiw2%2B7FR33eDD%2BTOhnsIgxt9wd0xZTICaqWgCR0LmM4mTHv6r2HAUPZjazKLusJkUwywEM0rZ14WYhYN7rhwMGIR6ken7l7XAnEQF%2BPpc2thTuXbD%2FcrLXPzQTgBH59RPirExcVqWROMdXXnNeBcHqtoF9Z1tbAT61NAC64yseYVrLGqssJC07XL3zJxK%2B3rrGt7iAJBzhRZjMKFhAbBJMvXs%2BKHla3EaxdtspJK%2FusPJekRriHeCHLbgWl8StKTWi3Tcz3fJo6AYgXI%2BC4nRQz3yePGrcZbOTe5gCnEhNpIjkwsh5T3f5zXO1qzVGzlIoc9FE%2FPwAyYfAdmq0%2FjCGvJLbLOmZ2ESvWLU1py6sCWN1EkRFhjuFDw%2BsdjOKozPgrJCb7FVeihKjgFJNrIhImsxIfQKqGVh1KvWdMbfqV%2BGS6xRI%2FRmzkEMeMLZjzFi0JtbHw%2Fm%2FGK1tVGPoSculGHTHmbHmBa2kAo9pwEGgi5fpd%2F4jGihrUyKwRkNKbfa078F1as3R7llN4lt57%2FpNmwLTzDw6ODHBjqkAUcWVV9skk8vEq%2BPWe8OtT0zTqedb0ITXo5dh64%2Bphc7YS9DpoTvkpBTyl6hNbTGeYWIPtzwNPSp32twn2xzO1gO8DzeSUzy%2BSnPAVPBYfM5VJ10blYVixBL%2FRlcgnqKXgImXeC%2FtUbL6P1p7MumsHgrBH%2B7mOkP56TfTONsvSb3IcViBN1R%2BEZ8C5YzPE7dJtJXgJHfOlbrVqxuVu86%2B49Q62pB&X-Amz-Signature=d5c953d64f9f3aa4ced5159c91b0486324c73c09ddc02bd90b9c13660ac74155&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

