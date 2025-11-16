---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VYNDAFRA%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T190042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDqHplyP1TrMCVgrSpaAAjjxytjTia9HImvmutmEGUUYwIhALQtGPNF1%2FLNOyN5IlEfH2NeMGDxh067mzARiH0QLS%2ByKogECJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwhxyp6KPVUSBvp3Xsq3ANGZMx8kWBNYvscdK6oV8HWnv%2FufoTm1YeNDfzbjPUyJAwicBUiWlWt0g1wu4EBdCENQE9ZNKcRJaYT%2FaWLS3dDX7L9vVq5s5ggPq9cn9ygzSf8pmh%2FOVAcd03uNAX1TuPn3ekg4rjSQbM%2Bv8OWYXKmL1GpotickKaFsZXftXekQikfAIw8hM%2BEtgvuq4uiNRzI%2BrxZF4buJ2x7TpfJiTMd6sMW%2FpH0kSxlWr6gx6FE4MhB7T%2FRehBhu2iXw53EQbZyi5wlw%2FJWtm6gcPzUgYGyL2szbeysFKqSGNtYNlp7p4zAFsh8og0KA1DQChvh07tsC1Peds2QW2X7EhRtC%2BAOXzL2IHqKe8DlIePbV2mT8sXv%2BTIQ6HN3nbYfK3wUaGrhACrWAlYG4QSlpIPDzaWB96%2B7fSqsh1Ofo%2FwMmpavOWpEEhfHQUfHNW%2FKkQnhJxEsHk9CVpH0kLmVuCEGGW1k02mq6qBnS2cMH%2BsNAgyM3g%2FSZx9Nm9fCF%2B%2F%2BKbbWirnRC632Ikv6TYA84BuGuREN1I0vlaSGIDyrK3OZ3UIFd9ZIqEaAaBQ9nXnDUEuzUwR5hpYTXhnNM5Kb%2Br%2FewZD%2BraQd6neivg9utZ0KtLIk3Vd%2F3SHyTOcr%2F6RSGTC13%2BfIBjqkAVk7YoND%2BKHjjN5RNsNd%2FNSeWfKtOikTeB0%2FKtZ%2F00sV%2FsMgoNLHcTM3PQSJEDk4O9ElwL85BbW38ICFIExH8H58W9f6%2FW%2FlauNGlXibP910cfmfQv%2FncSA4sGtVhCZAdgJPv20pHftdl9LO6eUjIlmmKWjKcKl4H3I4F27x5sPO1dEoufzPAc8ioGNCXEkvsmGpqUW8s7EoYIqDSO6oj3NYhxov&X-Amz-Signature=827d2ac7b827047b85db088c29b3ec47dcb9912389f90cf44e5248819920ebc7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

