---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XLFYAIAI%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T170047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJIMEYCIQD9MEye1%2BjInCnqzWpInA1z14rNneoQMRN5LrH3rWfyFgIhAIN2qFFQLLZ%2F3QYEkWR84O3u9D9MVKyzIfCkLj%2B7G8jJKv8DCBkQABoMNjM3NDIzMTgzODA1IgyG24G%2B%2FLy%2BKht4h70q3ANu48FpbJKTizUhNp9MeE1RE71ZcXwRIJWp0l%2FcNpIZousbbT%2FN9gTkIeBLN8pQKPCWzf0ZEsSd7vtOQkKz3ErGzfgAlmUBK6cMOfFSodiotW9a4poLHk5Jk5XmjD30iD93VB6S3OOJxI076ALjhFmNSUmBLXIoxWRK6PxtkaRCnPcYFz0VhYZUFZ4pYCNf%2BB%2Fsb4eaiXWjMhMT8b4MkBvQDDh9lofGZGlviEHDzZLKvfTeptvKmqpMxHJ%2BelseFLD%2BFMg0%2FqvQRUMuXEUvqsnIPw4994rMvLU6v6DKW7YSZbtATrGmJpNaJCf0DbrGYjmK%2BvlJnNN%2FVeUODGCUHn%2BnGRxXCvioIOi%2FOJYI22IPj6JNW8k4D76PuFAlc6iWzmvMgxjMC%2BZyT97Q3hR0kyCvBgr%2FJKRPwjJUKO6yHK%2B45mSUTYR0fsabgy43NZs8EEv4X2hY5il5%2Bd3baVpTGTnBt2fhRojwkSe0b9kMZcXK5ao2n5MlwSJWfAaV92ing3%2Bg5xIs0liUnppy1NyJATxoMgnUPyvYJr2QVL1UKd3%2FcbLs2537PxFVRQH0HG4gPx9cDekONLAxU3GrciqH7yVuAjm%2FvYqf7kW44nfsbPkY0liwOAXvpwTSabK%2F7zD%2BuZPIBjqkAZky6Z0q18w9%2FlvAAJyBnVs%2FrwkZt08td8d55oSAzgATR48HYPK7Hzk%2Fl1Cs5Y9VdUCyKvhCbLQBYD8N6JQYvAwhT01LvhLSZYBB1knH2uoEwiVN6ZWk%2BU9L0ph0hutvm7H9tQZVLx4eUizWMM2zEfhPraW8lqjZVxJEcJ5bNdhLfymgvm7X0NZznzzeihgh8o0Ze893vg0yFX%2BWPCxcNBefmE%2BZ&X-Amz-Signature=ff3547a466dcff2572b3a907a31e273a8010ae13652ebb28bfb0cfff9688fcb7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

