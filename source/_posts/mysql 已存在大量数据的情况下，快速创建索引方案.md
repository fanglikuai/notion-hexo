---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664QTX5BUL%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T120046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCBMquF2L3bZMOpjKvtndK3O2U5o3foR%2F2VzYHL4VOBHwIhAPt8AIKaKiD8t9oyrP8LXbz6TNt0F%2FM2y%2BKzFZCVOgFCKv8DCGwQABoMNjM3NDIzMTgzODA1IgzMowP2BJkG59oJsKQq3APtUQyE%2BcK6CoFmeRjrJImP12UavmDA4SEsAX5TXIb%2FMulsoqNuql9JTA1W%2BFWKQcRaI7POSBWyt4PXFb8FefPyKLDMuQQKd%2Fe7ce6Ak6nvcgQ8SA7OVDaWGOFgQgv53pkKPROl8H0sO8qh5w4dOelJNX0lgN%2BHRqPm5sKJJsVaYUr0QMmaA02ElKg%2Fv6fVhOmTpUIrNfysPP7CkYHYGsPY5pawQbZCLlZp4ku1issIderLEOk3NmYs1%2BSqE0lhz5YT5CtxcqLphP%2BLyNiFrCHp4%2BbpP3QNqPWHGkBFRf1NbeG6CK63YtmKASV9BvtyOPcgvdqyui4jr5hW%2FzqRQBQXZW1mtC%2Bmryzk4sT1a3cNtmiSr6yAF7%2B9Mq2nPiCjF9LbrX7WgSiXraN7EtMYXqyj3K64hyBxNbWan%2F7icLKxK4%2B87%2BVHki9XDDLGYhr%2B6ZPfUdDp6FSvCBhQFxjyZy97IfXkMH6wDF9Xc0zA9PjBnv3ErFTHgGz%2Bawze7I4B1Vord8aIEpPnX3ZXDZOrdRGNoRKL6L8KuZFThy%2FI9yf2eHiQEvQN9Xqu8tFM6Ko%2FsJb97Gh9eIxgoS4JyF1u8k3nlLnj%2FpZhg8toIFrEoKe2v5hrMJJ6Ar4ZJVaQKjC1o5bJBjqkAckLsC2OCbqZnez0K1ozgaUVH3Mjjtx1nQD1BnYGHF6IB4zfYh9RtCIcR4y4ZAYPlPzBnso4i3025uRx9X7BAwgBJWhJmI9MGm1EgCC6tHTqke58ilMGqXwx7pG5UkCvnCmfpfShI18IvNiOH2vabrxSRWyJ6npsgOZ%2FNV3iK33aXg2uT%2F1TXQ7bUG%2ByvS%2FhEwIQxRY3yBJb%2FkjdGN%2FnjuRAmJom&X-Amz-Signature=d869aa96143312673addeecb37b6870f7688bfb564ff514e79edc4cee466accd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

