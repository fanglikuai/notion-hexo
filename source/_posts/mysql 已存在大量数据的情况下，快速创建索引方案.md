---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662AIK2T6N%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJHMEUCIQCBpqazD5J5Hh9y2ftccOcjcegUHXxoLOOyQMCZz4Xw6gIgLJwHkFtxuntXkFbStrW6d%2FShESZZLtV9atINjAfjkmIq%2FwMIPBAAGgw2Mzc0MjMxODM4MDUiDIPbMEb9E9UKG80Y0CrcA%2BqWAnZKCF5vCiC%2FisJnEueMN8Yxe6YvEiSj1Z6sDVmtbxArsPjwvvjT%2B7qJX2OOzb%2FklwozDtgn0qr9lf8ONlXONYjd%2FWt6iRn5GEv8VQS%2BGKfwCf4kkYnY3YtzuzzJjUSmboCyy1xFt%2FiZ88lc2WwY8Nfz1Ywaq4Y8oiNaY%2BZcKV5QXuB2fgidYVZs4dpQ1scjietk8PsVYLqCl99m4JLp7ncI629j1sO4DPDmpLLI7iyX2n854rBCcEzYzvbOfVOgTx7iy%2B0IBfSvqivYBmPHzWKlUl%2F%2F2BKq8Lrt0Pr%2BZHhUwgxv92lPmyIJuJIduU2TFMyvXPgj6XVf22ltVY5y%2F0c%2F6KBmogRAaXnerhtyF7Am3egA2fDAgY6zhpp8YiNZbmiVajtxc5s%2BDzsYJdjk6zlmCM9EA4r1S%2BZ%2BAuSBVnAZRMn9WJ1dNVr6HsLe0UwirrX%2FKEl43vsaJz5r0RAJjND0kBCGp8ZlHYi%2F2mZE5Bl8o5JnCgysZcMmjnLvD60IcLVxEhqAkIz5qhuSwkscggzpxJIq1zifWI5xtvEamcO8VYU1LSvBpvDM2%2F3ZNNAQnREY5boN6g%2B70MAaqGAWyQMvbuHoiRzZlASpzDKDy%2BjQaZBDFzBqvJ%2BGMOuUm8gGOqUB82JCyCRLYxyTA3D1Mfszl86qfwblr5itQDg4R3OuDjpiqh4g%2BFLSutAbjofkqdxYwBpCVBU2Oc0OxWFhQvun6aplCc63aDJfBy4tbyYnrO7wudEXFmoi2AmzYDjsQKbVBs%2Fs126kG45D%2FVphWU8PWTKyEvVvtZs1gE4bfJeP%2Bqh7fdaweuk8FQJmlBoVJnSTna0x%2Bvd5xTkBJxiAy5%2FQFt5%2FShMr&X-Amz-Signature=6db37b68bde1fe405ffa34049a4e4f5718515aec4cfa38f693c388fdeff29421&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

