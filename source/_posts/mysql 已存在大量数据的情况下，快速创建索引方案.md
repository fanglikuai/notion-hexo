---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SNQBNIDN%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T160049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB7em4BMG6rht8gYjHzvh%2BzwVNP6UFqF0hFhNYZEXiv5AiA10fw4cOGaPMIo%2FTKxMmaBIySbDDPxR5yyYYdcMZB5XCqIBAi2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHMNTdUY7TOQWTOAYKtwDZIabvmj%2FYOQ3H1M20GhZCWfKDW8Wp4kmm6Toea8tY3SD9Y0bw6%2BPfR0CC0HLXzsQS38Rw%2BYdlX1slX2zqfRrDKRMOUmSuwJ%2B8yq8LTdcJN48QhdcSy82muBbGLLO9T98tjtYJBDL6eHIgdvO5BX4Qp%2FtfXr%2FXt1ZMAMxcB7UtA2iXWWs00d3bXVZ8hTz80IDCkd14lINg1l3OEqQMESTzjIEvXXoO0eNE%2Bc1ZnJWEYyJgXiNTU70Sjeqig3sjZvstB2pvu%2FucJwbyWfO0iINeumna1z2ue0C%2B2IUmDoFKMCdGpjtdXqqlJwlUrhokHigMZUV%2FjZYyiwY%2BPi8TO4uaa1E3sQgulRTpp7w3sMi336cqIH4boc1vL1bROf7GIYEWtThoub3u1grdl9vP4Ygh1VG0eMGgkSbjn08ZtpSgRB%2Bi61NZLRiJVVm9C43vNG5YuusO1wz2Ycf5bf9RJpSzV9CRHu42nMolvQ6SQJJ6PUoMNAFCzZgsqe7vNb1JOc6WepO4Wji4T16FsJgr7c2sxA1EVcbwjUNU6VE5gjDoB%2BuUIJ6Dgp7Z4kYIum6b0HTGyO7pgIBUWHqeat1VkYQ%2F7%2FJUMnImjzczjXYvWYvtrjJXccAJ8oUdOteUSsw%2FL%2BmyQY6pgFF4eeTeWKjcoN8mUoPHiytV3%2BE3e7%2FVxAkHXvtPV4lxVsDmXWa8FkgDYvHkgn92FdJBZ61ViHFwvaMZSMxjm4ODKjz80N0CsBWdgCpyFMPeCgFAp%2Fk6hzyaXTab1FOrXxeebi3gb0OkzIXpv%2Bx6wX0ewHW4KE%2F1gi319gczZ7TMnyBPVcFcCslE9G%2FTz4Y6b9Lm02k6tmpDog8xEOsPe7hc1Z5a50D&X-Amz-Signature=f3cd8834966e3061cc23b083ee4f843722b28ea396cb32be8eb753b40d9fa3ae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

