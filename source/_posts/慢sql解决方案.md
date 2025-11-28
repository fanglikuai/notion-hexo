---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SNQBNIDN%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T160049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB7em4BMG6rht8gYjHzvh%2BzwVNP6UFqF0hFhNYZEXiv5AiA10fw4cOGaPMIo%2FTKxMmaBIySbDDPxR5yyYYdcMZB5XCqIBAi2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHMNTdUY7TOQWTOAYKtwDZIabvmj%2FYOQ3H1M20GhZCWfKDW8Wp4kmm6Toea8tY3SD9Y0bw6%2BPfR0CC0HLXzsQS38Rw%2BYdlX1slX2zqfRrDKRMOUmSuwJ%2B8yq8LTdcJN48QhdcSy82muBbGLLO9T98tjtYJBDL6eHIgdvO5BX4Qp%2FtfXr%2FXt1ZMAMxcB7UtA2iXWWs00d3bXVZ8hTz80IDCkd14lINg1l3OEqQMESTzjIEvXXoO0eNE%2Bc1ZnJWEYyJgXiNTU70Sjeqig3sjZvstB2pvu%2FucJwbyWfO0iINeumna1z2ue0C%2B2IUmDoFKMCdGpjtdXqqlJwlUrhokHigMZUV%2FjZYyiwY%2BPi8TO4uaa1E3sQgulRTpp7w3sMi336cqIH4boc1vL1bROf7GIYEWtThoub3u1grdl9vP4Ygh1VG0eMGgkSbjn08ZtpSgRB%2Bi61NZLRiJVVm9C43vNG5YuusO1wz2Ycf5bf9RJpSzV9CRHu42nMolvQ6SQJJ6PUoMNAFCzZgsqe7vNb1JOc6WepO4Wji4T16FsJgr7c2sxA1EVcbwjUNU6VE5gjDoB%2BuUIJ6Dgp7Z4kYIum6b0HTGyO7pgIBUWHqeat1VkYQ%2F7%2FJUMnImjzczjXYvWYvtrjJXccAJ8oUdOteUSsw%2FL%2BmyQY6pgFF4eeTeWKjcoN8mUoPHiytV3%2BE3e7%2FVxAkHXvtPV4lxVsDmXWa8FkgDYvHkgn92FdJBZ61ViHFwvaMZSMxjm4ODKjz80N0CsBWdgCpyFMPeCgFAp%2Fk6hzyaXTab1FOrXxeebi3gb0OkzIXpv%2Bx6wX0ewHW4KE%2F1gi319gczZ7TMnyBPVcFcCslE9G%2FTz4Y6b9Lm02k6tmpDog8xEOsPe7hc1Z5a50D&X-Amz-Signature=1b79da82c3ac43574d7ada3f98d31b709cfadadef36c6529344564250b848183&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

