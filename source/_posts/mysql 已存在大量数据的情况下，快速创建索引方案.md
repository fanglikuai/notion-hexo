---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46664YAN3FS%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T040127Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHQaCXVzLXdlc3QtMiJGMEQCIBEGnFOY2ha4GKgah29dAROmsDsiwt5p4Eht5WhQK24AAiA3vO%2F3FgoxcoVBhirOscBSshoUf8l5KyBMyVWJjrBAsyqIBAj9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM410HP9vWAwgxYT2fKtwDTkt45dPOWh%2FzTCtq3y3OoMkwOjcl%2FX%2B3FSmJq6MYkdUydWkPUsTDmo1EVrWN89VPE0o7VntooF5yGLLBMviyxkh06m23iDmWaNIxX4pc46xcYTOez6U9QxxbwFoqu8gEFZcD2AU5JNEs9HUNz0JrS5Wu8JsqTROFlLAkABxIiesc%2B8bow4wq390Wuf1X6FLwMtXPxWdyI6jrRfWyWbsMFvV0Py%2FcOVYGkSf1lrCXjXEUT1dwPJzEbat9TQ1jIf0zvzEKIAmR2S3sl9oi3wK1wXRPPc6T42F7LeKVwITcmLyx6x9HX9Z8U4MxcODJ3PKw1PUtm1HXEm0vg7K0buw%2FBlbqfGN3O5aeBdRxjcWZ7oX%2BJ%2FebXKMYnMH8lXpV9nkYoDp6hSjjVHhqLGngldwg1KY25q2%2B6Nblem358OOTDqETQfnGpUMFQIHiLN2nvcYVm17y%2BmWnKAMaDmUByN6bvn8X0ibky9HaP%2Fi0fNtRH8jXcsGg9kLM7%2BHS%2BbXuSTXkyfjI7gVwfmIA6HlmcVBm8M%2FsOWEen6PlgaL6NxQgQr07wpyJ%2F%2BCBn5IZQMoFsDHaNRbcqLw08jK2UN1vvTuorMh%2FAnFWcOhDqfGsRUCnRNoMv4kA9so8lW73GkMwuc7yxgY6pgHhZHUTuTgBfkIBTdEsyrBk7VZ7a5CYCi0KxankWgSn38WCawlsy7yJXpiHnCN1cr9r7xbOJccFV7h1q6oNFRMlPShAv4rz3Z3Gk%2FRIwIawpLN%2FbgiulDdqvqiRidQ%2FXNnPXaFpYEr0aF2oxAiQQCjOp1Bg6tjQovIcUB0Ka1azOQsCW7oRZXehvj2zCpE1HdpAKHGRGhhS%2FWUaqMIQTjMgE0dA8s1B&X-Amz-Signature=880eefc89f372294451d8b20407f7ca5c7eefd205f509318d29f25a92ed2d86c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

