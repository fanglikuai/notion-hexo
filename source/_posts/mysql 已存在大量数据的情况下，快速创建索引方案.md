---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQN5Z76S%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T000042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCRJPxZPWmoQb1tNI6sOG2NtukSvGIryYKgcehcv0r4vgIhAMgiPHcR1VzS9v%2FT6%2F3DBXqLIWEWuOHOiSAaGEsd3iSOKogECJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw1nsIWnTFIyeMbdU8q3AMNpiMP7pKpZ2wm5e9iS3KTj45LPLXmUkz66LO3hTx2AkGXoOTR%2Fn1byz14shlpswsl0M3OPsqNSLP3KQ3PwlvNU8cVLHmLcXyEG5W3JFhUmT4lBhLZS9l9IYaFqoZUlMuvJTBgJ9KbbUREZ5kCYZK0gNB3kxjv0gksk76q%2BaimSlMI4kK2VAw9CqSZgRjLKSbqSYZrHxfBHSNHMB1M0wSChrAkvApzpyFWRL%2FQkEjFE8dwdbv9bpEtR8S4BETAfLb7vlIErLIJXMP4klto7S71ci%2BSnBNpVI9O3b3%2FtNmfauaNBtHy3rME49RSvREROYb%2FVfRzzM3sj7wE55pjbA0pOSXolYL7tTkHNdpZeZezuc80tFlsh39nSvlNlEym3W4YUlDlUaM6EnQ5RYt9hmZV5dLMt16RztoIo9xeVOyCvlT7Y29lMgDP%2BxXkbwQyXXsfgRgn48IzxCkNUCsQuzRn9OLPeJyAT9xYrTr7QUglrzpueCeGwYWjSi1k1IuyUGLAqbrFPN0kJM0eCMVB6r58RJFHaVZ%2B4uXvdObBnFWRJ1GNfcCbi5gUkL5cZHG4wpxb2f16NQZlyJXxIj8NbczwBbkJUC%2BPUISY%2BOvZcLQ6EhLwoHUZGBpAPNng4DDarq%2FIBjqkAUvoPA0gJskuJulESedXdOXYoUpWVnnYTjnOuxy4yCxIbGxmXKzLBHhyOOte4TvExieya9rYpS4%2BQdGHFA28%2B4IwffaXGiL7xkxLmS6kGGk4SYuXi8CkoUcNfe5u4UlwmBzFnrcm4DiCsySlvuRiQgUMubS1HQEmld5phD5YDvw%2BkTZw05yuLChPxjwHptCNkCzEmGIB8eGYd4jqs2%2BJ0HEMlLrG&X-Amz-Signature=532fdcbc6cbb1e6873bf95dbce459529a66007184f326dca7006855926fce5c8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

