---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VEC75CB3%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGoAaE10S6AryU0VW3O2qGE34sedxXClT%2BkkCOGeiJftAiEA6ff5%2BGL0IWL%2BGmkIqpMZLS005qKpBb4XeDZf6BkrgHIq%2FwMIfxAAGgw2Mzc0MjMxODM4MDUiDGDbOxXZQmb1Fy6RRyrcAxrrq%2FCGFdwHx7%2BY9fQ%2F4h%2BdLygxRhOk1gD%2FpWUT6ooZfcVUWSLkC2btQoesOOGV316LyC6cNZRpxouyXrx2X2xtvj1uyUL%2BlC9LW2OP%2FqbOfuO9zpibqz5cmf7wZjyQSC7vO3l%2BHToRVrqn%2BJTqa1jBIEy9ClAnvEfwItHhbwZ4LaiPYk89HbuNh93ZirHez5Kso0kG%2BH7PW8m7Gdy9QfV5UjK5wCaJdZWCE%2FHvSlDrGFIzFBqrJy111JxI0f0vdt0yLobmdAarBWsIwrVarhT4qszApLEMPTa84RKpzeUWGo4VJqJZ3ymIDebPVn%2FlWJLuPnB0KT%2B2enlAc9fdOA3siRrRNZCnqxecAM9K7%2B6X8FkDfYmEqyVpM5rJWtY1O9n8pqkYMpxPOEAEHqCDAysLyjUKvquJ8rcyaxN6WhldxejxxP1V2gMdnZrjJSYqdBK5De8jnRAsI24ZaOw8oJNLZZpC%2BZIqs4NPinf80%2BvS4Wb%2F2mXD2%2BUl4RS7uAKrV7s3e1CHorfbCche%2BGjTcfY2mOGQsz%2FdHxHehTEZdOWovxQ0vfmpo%2F34Xg6OzvVMDpyomg6u%2F6Xi5wXcqgQpvcIqoHJBQXJijizvwHMvqoJd9l4Cv2GNAIhWTFogMM311sYGOqUB9mIAYbvl3gqZR3OUfjEkX%2FQ9hgW9S%2BLYFWBxxbZlicVmmRlQwVkJ7ViJPro7%2FnRogSCncap5tcNq3J07Rg8HBcLnhNED%2FYf4TMGL%2BVFJAtT%2BOZAux2AV8L4tmdsv%2BzWiLt2djoVoctf6nSu2NHgjaHD3wxqoW0%2BFUq8J9JFaCNmltx5P8k0gizdUQjwM0Lty2VPyy0GbSWPpbtEXdM19NVjUe28y&X-Amz-Signature=e8a7055e02506fa6ff5f9175be943c9f18932db35c80d23db1ac91f1e2784fc3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

