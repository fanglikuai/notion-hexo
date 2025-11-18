---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XRMV3DBK%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T130045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFVXkWQ4c536AA9UCnFDJL0N60kmHHYtVSJC8cGoW7yoAiEArQSSHQ%2FjWJTEUFasMroeIFarNZBXvQyS%2FMxrKLwRV%2BoqiAQIxf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMrSqhDcBa%2BSGRc1YCrcA3LFonk4LrWU0ZEC8kobU6AOosoqCPTzKxlxWEeHgz%2FMHjI%2FjVmoPN0eApBhff55wdMGClww9mDE6T%2BG9lxvG4J1I5qZeXPrRJnykdZFIjKkyVscnif7Szxi%2FAXtpz%2FZEngi2rm4JRPbAL%2Bl4EJzR284dfrkXvxJoh%2FRk9i3puK3M1HLtVT3i7h%2F9a8UD4r9C3By3tZfqoVXIV8Qab3RrEr89DwNKrbvexYoehcCW0Te9YACylfsxE4k9vNPQhula7ZllFGktOw1CfoZqZm9tgfR664Xs1PvDBIhy%2B81XiV1iIBSJ2YKhzqAPr7hDDySfIH9AvyBV9gha46bs%2B8K4Matr7m79ogWVbHJ6cCJYLtu43uLSCf4%2F6c%2FLBVUf0JW7GOzlm8q9q36%2BQLy24eAXkv0iY46AFp%2BKf3x%2FcSK5e4Z7bV9LA1U6gR9TVm6mMDfwGf38lBMFL3TqpRAnpT%2ByOSIfYckaxn%2FdtlwNCAt7ZEe5x71NYCun2fU7zeCRSTtOjyItimxUmxjkkmRNz8OSogWb5FKp3MHxKISMGAY6cnxAbSXAtraNCXIj2eKZFB1sUHHe5xVzkWqIqQakWoPCiFs%2FUt7xRvBdGgd%2BxYzv0f1z1zmMtyOXrwiGr4QMJzF8cgGOqUBiz1Vfl%2FsKStKwjbj9H%2B%2FV0UO4STJtpDv%2BgPHNpC2F8pZGWlE%2BlxV7YoYyCdTH8fUti21rnx2M1dMLcz8mBmxpR1CJfYgoQYO2M1fR3sYMia18WZ%2BrMBbfePtshyC92StUpNXvYJv0o2FcvamL0qCi0WiAdUPNjq22JfduGH3W14XWyCRInsdj9AzhU2h8rxgq7ZJK0q1jCxCNrrf%2BXntuEs5mkoP&X-Amz-Signature=4d673c64377866d50e1aa6472c4c77a73e5b19ceae31151b2bea00315a9e231a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

