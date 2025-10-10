---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TDPTS52O%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T200045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJIMEYCIQCQH8UI%2BwiI%2FLrjk4SPxZH9CFoGhpiRMepE9hZHjzp6jgIhAJgQjxzrscF%2FH011Aw7imrElpZfAsh2gzbw4KLCtzwOcKogECPX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwKOT9I9WSbTks%2F2CMq3AMZuAH0ypYQohYh1iLskM%2FfUdtxq2qurSa1gH22B3%2BvccuWGHdxMw6jqEBYlA5h2xAUZMSvN%2F91TTwGrpkJ2oeWRrGpTsBT2qJwHmvC5Tq7%2B2tPrOq8J6KNQ3sSQhqfcpv0ILTHu2GlEt0yII8yPx7nsfDM6XjJdfsvwOWg%2BQchcCwsTIOykx2XbJ1vlOfsBiEkivqR21ikn0GOOfKliKjcNgtEkCJn9AEBPCzfBAFDMkXN0uqiVfYUtduQVRKcE5JSItIIRMhAWk6V8r%2BjK9Pe%2B8HQaIrzCl0YRL1lHFG7gKb%2FF9%2BXvtkfM%2Bwz%2BmTqnbf%2F%2BoVshQvq6yTLqIJhSDLZLYvAPl4sDKGAcRX0jN2NXpDCZuPm7rQjGVxd%2F18mYYmwJBS13njqxR2jHaGkog8eXzzf%2FjSQ6qe9smq3SzSOlj3xNKxqF2s4LWGBaUXCYGKgbFyIFoMZI5mHDVQKufVj%2FFYfPJuzVRyVYYh%2BkJoqo44zd59T8UdSawAoGk8LuQXP%2BOKof66Z4Fl3JYvGBm7mT3qCZR0dPBQ4XyFZoG2Y0v79Wn7fubtO0BkQX1LiO5%2Bs%2BVOR6KBVXbg2bjDJhGbYGzPDBzTx9ahjHS%2BIDFGhkiWncw0GwPUjkWD26zCIvqXHBjqkAbtZW6oc3F08ArouNhrPda8w7um8kZ4hWWfnmpAl5MrPRuPzAi06zqi697k9zgZctIizWUtWekrWNaf3Tnyio3ZuPVibtS1RJmoJHaAAdmyKGbmKsRW8ATWfCWsnliJ4SEDhL%2BmLQpVZkQxPVw3kqlNZlUfJjzCIEiKl5zn%2FiZM693Xpxkj2CbR%2BAJ4yNidx1WP6h7sOt1e8zxk0sUDMjufkaMhC&X-Amz-Signature=c80ec548c8b1c4effb2398af001aba12e14d280b8257cee494ec911730ffb218&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

