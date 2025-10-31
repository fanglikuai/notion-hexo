---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667UEY5OUK%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJGMEQCICzWNH7bP17WM08a232DXsBTjzGSXClhZm9PCoM1mt39AiBmKKdB%2FnweXp%2Fpy8KToBmIhUEzy%2BkNWUHNmcldHkDFWCr%2FAwgSEAAaDDYzNzQyMzE4MzgwNSIMK05UB%2F5Vb540EZg2KtwD4w0iXHVeMjYSA5k9FeyA%2Fm%2Bl9sfgrCzeSw8PRsYl9%2FYG6MxQB2SR7WsMpXX4YZASUVEOOn0R%2BQiWRKZXb59dxNndVZ%2Fr14aOgGZdeRzFZ7CX49ZtaEfNJLJ9lkuHXVJWhyIfey9aCXA2SBOLE6bN2tvDVjhvzJMUmJ1QZUK93crCPCqIM%2B6Uy%2Fd7wfctoQnE9bPqd7CuCGoFsSabUqZkJ9zCx%2BmICE5o56Djv%2F1IOWBB4EyOfyspUVxnKZUg3bX2Gdyn3o0hF0JpRpi1hcS2wi8sLBMeWQoPBqj4XSGtEyx4JdlsndKquZdesolK6mYNZKsKs85Yv5GUuxEen7bNvDPsr0KGm5U3LpLHFOhE%2BXbENHLU72igfEm3U1IIwCl6tmNzdVOHQexXg6arcJSFe3ckIb0JjC8bzSTVX5pgPiCa%2BxFE%2BwvijHzjTBoujdNNN2lzp0RbTTp9Tb3afRiYXztzA8OUYHHiBCa51Opjgz3cs4QYKDQK8Vu8nVsuffsrSvsnNeAoVzqF6cXj6gwGSpMAa14e76jh9gv9OxFPKd3Wr75RvdPJ76zjlC1afAwgMH6fqNKrgFfAF%2BKzSwjZwOJfawgaKukTJbPzuG6klTUhpiESkyl2yIkmK54wyeqRyAY6pgHiQaH3ruyzj0riYC2yM6VsLhbceXsKxmBA88S%2F6WY2B5K6Ux5Faer9tsY1QRN2vIDEUrBlGwLd%2FjjR%2Fwn%2BqtlA3%2BSQy%2FOcHSpHRqaUD%2FZ%2B97ggV1Z1Tt5QUGXA%2BZrVPBbBcACYyh0PUa6pNdfe0Gg2Zfmg4w2ZHatGjt0V8O8lfvp7GinQeMU89N%2BAi1jSD6fF253pMhitCo09Q402i83D%2F9W%2Bo255&X-Amz-Signature=3953f748dfb700e9d3fc27bc65c83a998aac167a175e1ef5482a8e5322635e15&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

