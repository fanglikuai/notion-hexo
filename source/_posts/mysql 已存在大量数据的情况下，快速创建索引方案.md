---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SOQWMDDG%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T020052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJIMEYCIQCQGZYemQTgo7AUw69HhRe7TQzY8rlvV9sqzK3oN0wcbQIhAO6m7yN8FHbd%2BqfQc3eMM7OSjsdV%2F68RqerYv%2BDxgcg5KogECPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyJw2sl1n2cdriZ5MAq3APH9TRyVqGqk1zjER6r0MuuAh6KJTJ9gXTDFCr6GLg%2Bn5X%2FyJIzzJ%2FaRuMnSA%2FqJMhtGXiSW8Edj42tKrR3gw1k24x6K5nt3AMiayQ58mjWXXHsU3BgRLQrbybFgNEIbDsbrDOxmROaohaw%2B4wQsyqpO0PzvPKPy9CV204q0GECgKuiyLKiNg1d71NcdKV%2FdYf9bYyUWo6KEdpBYIR8qkytzhWVaY%2BdzRkoqg03REl8eIR0g84BzrGlJdgv0X5D7nauGL6YPhuQ4C2pC1TNTz%2BKjWlng7PciON9iS1j1N%2FSLi3bIUnz53GkzGS76j5WZbkw46hOeq0ugHrzYyFwyKjpcl3X%2FknNCmjS7YRICfcSgm7Dku%2BqOwF40%2FhzgRVEktNzI7iTu8MWHlUEC1E0YGtZKCgKL4ZOCbyMhryXN9FfqcsRYj%2BwsDLqcgEx1rm0M21dnS7wKkfZPX4Sp7bJfZH4%2BSg2l%2BJLWcxYd5XfdwieronHnhCSvZr6rvh%2BU7IQu7F3rRxG%2B5xuG1RpRJA5WmkIlkoHP%2F1KL4tYgPgKQY4Pjxf5Z7NI5v9gRc%2BGTpyvWXJ7Z0qzo4lMNvtuztroyD6mjZGG3QUs4Kqod9q5GrKpxRt3WoBRTSiKOz2ebzCu7sTIBjqkAdXrE%2FmHVYKF6qeXZA6DLIa%2BnkQgKNXhQofnAQNAHocvsvB2Uahf%2B6digqAlS7xbBAoGuygJzb0wIYoS0OATTRpkaYSPkehUKIA1WbiErt%2BTEHAJZY%2F6EuXUxZttAiZJScb95Yt9k1oT7m0XYpf0dYsVuBW0hk5HyMvTe7WgGdHalaD1ZXILYZal%2BZI9LkD2OlJiV4%2Fla1LaKNSm4ZFlqgsEzJcv&X-Amz-Signature=2eecff59e434fbd416e2f23ee3351cc8f54642244e088aed8383de9b1a572806&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

