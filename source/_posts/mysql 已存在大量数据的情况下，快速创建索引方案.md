---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46626QYNZXB%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T010037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJGMEQCIH342%2BmMHkHQ%2FNO7k0bU3uKDIx4%2BiSpVLDvLIc5HxD14AiBTSeE510OIvgC7wIOGBbmN2bRbEt8eCVf6riMgw1LgGiqIBAj6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXdGiKvO4IEgO0sHbKtwDNr5eOzJUn4gj1FAlP03yaeHVS8yodHT66e%2FlEx6CUxkc8jU72ZDQ9tG5Oa7P%2FRYGhaYFKXXx6cL5vbWD4s%2BiKNG8aAEJo3mREjrdfiDYcRtT51srQteGPG2nW5Wu2miez8Pauh2af9hAFTotOVdW0gN6Jh45PTriVP8168dpYeC0psWJ0oT6iK1BHQwbOI%2FNAq0rmvwnOJdFnoAWdQtAf%2BRFoE%2F3a%2B5GrXhCC3rVpdQodactjr8OE6X5JELiAdXOUdrWj2sv3HYBmyCV0URT1Vnv7ojT2kJjEjObIqaN%2FedaGrjxtmFOJzCL5JYgZHB3wJcVdql%2BsPk4eYUQ8nWuHGjE4J2yNaagTCgjwPwZClafJsWxXPqbg0nup1NqnUYNVg16yyJvFAyw8yA%2FcDRvN7tvTxcMu5vcFUu3beN2qJzm8BR%2FXpxZpZdQKVDa8fjHsMWOCImnj562i5hO5JGod1JX9Ew1I9%2FpELrn9ckg57cS6B%2FVFgX6xpgvE1GQ%2FegiiHcpLXut8pfm08Tfonks0PEJVSwvNfUB9vP9TNli6jYzn10VIVnO%2FSz7xszTP5fqmfK3rpoIOwDZNUj%2Bdvgaf75Nboa%2F7Koa6L7mom6pEJENReXuMPu77E2O67Yw1urxxgY6pgGLnHyCaVAortU0Hl8rvfaTJ99jivoy6GofaDAlbmZr3kv4JXDGjBc4UNuofECZDGT4nYwoxgNpsjAgPu%2Bo66Nuy8zfqfH68apqRALh1m6BgSrKpk6FkqL0%2B0RPFqgurAhTmI7cvw%2Bocrzhfz4lUpfQtfUZywONr%2Fg3ObFWOES%2BQCJsA5ULA5rx%2BcbNRTRO2rviFgL2ERzN2LnE6MSqhKO1vNZf5cXW&X-Amz-Signature=10c455eb25a47da691672dfdf2ff3bfc67f572cb58c62c63f21e31c4e4231d9a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

