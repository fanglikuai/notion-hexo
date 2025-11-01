---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46622LT5U6S%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T070045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJHMEUCIFRmdC6AL4%2FD3%2B%2FuQcqV%2BEaOW2aMk8HL6KMA5eeG%2Beo2AiEAsqdXaJnGtu1ZZKQPUdTlHUqfitCUeXvMzl4J%2FeNuWhcq%2FwMIKBAAGgw2Mzc0MjMxODM4MDUiDPx7D75deUsDBkLicircA54spyqsbQEFMQXu5Q4h36KjDThu2edEmV0rMQ2oTFItpeSnKCZTAQsl24gWuqXPNYG1G13FtxDdtGjsTTN49IETabtBrxz%2FgJWoBjSVkQ9UhbLhoefjSnjU3YWzbpJGsn%2BR6Q%2Brx3aVdBVwBrfstJdL%2B1Ft01E3TX1e6f%2FW1YmN7zJz%2FljCUhmwc85%2BocJcUSPgX2ich3lAMbHek14HovCEa3oCGV50tTHCm4v33sU%2B8WzmxJkp%2Fq6TZVMC%2FZFfOvstCoomCZ8s8zH8m6W5HH4vGkULLRYa6p1hK3Kw%2BdWE5Xc61cKTTA0GYlOYBKZqKYiIjDqbGtPuY99%2FXOr1V6e9lQG9pp9rtNoa35qkix%2BExSZRZE3uy82wik53eG06URLSjnir80MIMwjdvwpvYXDKcN9FdP5rlhDrLGPROefhW%2BSs1wqR%2FRTTDVWZwNkPhzbx%2F0n83ATMJoD5MJqOjvD1fm%2FPDiohM9UFoFwqsAFEWH2ZjMWhAGritFHFNXn%2FB7tNCigovD7lhlgGhYjuhyLik0YXPO92FKz9V13CAlYnGV55AkO5UZSNxGPTdWeH7lGPVNN71vfX4NoFSVcqPRTdsyjKtKHr4VBJlyjOkXgKdJQyhgiydOzgj2UjML%2FQlsgGOqUBwRvi9M%2BehwF%2F4f%2FKHiYmz7TwHmFpvEk78M8To%2FRl2cZg1kW50v%2BIQS7anryQbWD4Kfv1A1qEq5fPhFEsMrIlRj5%2FZL3Xq2huB8G7yrxBJJ%2FOZvYMCZ%2FoYoVOlUUXCpWIV%2FKTuo0XvsX1%2Bzwu9OdpnRJGF%2FhcX7qMkWQqlIYQ%2FBukaVEtvBEny%2BLTgrUiqSjFRRvoAWLxXLMBtGQFWcb5QOSUzczu&X-Amz-Signature=1e5cc1c0494ce2b3d4d32f83ad064574726f015280c1ade060dcce2f7c994439&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

