---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WBJDZDNW%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFnW2yg19WMiMvSrBDJ9J3dBrdruNvimTpiutklEGSXHAiA3fbACvtk50vBLNQfGMuLWUi8QoUNgDok9eO3RhK98Yir%2FAwhKEAAaDDYzNzQyMzE4MzgwNSIMvXltWVjmodnbwjppKtwDt7NEbEmQ%2B3UeUOF9eM%2BBAuaMJ9H6aZDvFhJRs9ezVLZUzPv8ja4dvcz6NDISll5WibHGfaDfe3Mle8QxH80%2BbBfbtm%2F0Mra6xtJE2k7JhhhRXAHSgHUXMu1yQ7CXAO0aN9TkHE0lZ7Apxael6UmSVa0HDwQaUW5o3HqdgpI7ZKKBxB%2FfF4oVqBznn8weax%2FKT9DeW639lo7Mey6Biy%2BXEaDsVKMkFPtI3D%2FA0URc5NuMAZeyE60xUP%2BpQRQ7XdMJds1M4AZHGbJoBqJaewzvF8UCp4d2JZDEXG4jBauKYSxYiP%2F7ZnlhhcqYrZh1SVrrbVX%2FXA7iFpiM6rhgiau8dXk6pHUuQVVQK13aIE1ucj0NMplYFmYDoKNv%2FazpZPf6r%2FZK7fcuwrZ%2Bpj74WyhSGUeVGxOQZfsvYFBvHarCdjdWGBaswpxSc9OPra6pDK6VjDrJ8vlO6MTc4BrHBDAjzPqfkgOLyjrofNGWJUQCHx6uK6M5UNqXiCjMJOR3dGtNRHEo%2FoJhndOC5dTF03Nv%2Blo14geXLAb8yP1%2Fcm%2B8QvgfG%2FeZlWOcGQlN%2F7a0LU8o72cYYcIMFfp4hqzWw7sbybhNdnzdpE2Uv4%2Bbz6lmimtBH6pmFbdHT1ZqcHIw4KGeyAY6pgHUM0ggAL46QkixoUgAIakhFJsceIkTyDvs8Y8cjD7vvJzHqDqyJDupRgI5fq7XHHgxwdF1VKmhZ8vlaK5Dopbb0r3n8YFWNbUGIF1TK9kWCvmsz6RHi8IMUoxpXZS5XV3cLsqHGImI%2FJjO%2BfdFmM6WQkJJLGjCfzy3ORdKvTbWWQYLorD9uMrMpqitfKItFcDOqPmaaG6BiY7hCYB4bmTq6JgkWVzC&X-Amz-Signature=61325f1df6f11cb3775b0aa3fefb78baab64959fff5413e0034105be869d7831&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

