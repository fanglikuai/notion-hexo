---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RL4IGOTH%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T200046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD9mc5BKfrVe6tEUIBaX%2FTaSWXC5S5Xxpafthrr0kgIlAIhAPBc4KnVNgCSvmHnr0KfFf8wkMt3bix6efcyRO2sGEbVKv8DCDUQABoMNjM3NDIzMTgzODA1IgzQJKgtxOAbrNPrpjEq3AP5d%2BAxJe7lZwqlaYT7GPbmQKeCHhWMNiIocSasj4lLo4n7W%2F8kd1SZdr4Q%2BDVNBu2EtdGKHC4qpLot26wTAOKxVUW0eBPG%2BV%2BsSZs%2BOQ7NRubbnCScWWbtEr9kFcnAOtTkfW%2Bdxkt0YS74dfMACcBFihWagedDbghd7Tb6vUAOHtO5hCXbwi06kIFHNZaObeLAuKcI%2FDU%2FT6WQAZEpujaSkDZWscfl0fFtPG7%2BI0FaHIndBbbGOpcSkp03XwAtyiyndAbD4b54oz4bZ597joEAs7NWLQNe8d6bsbWf1Y0t3QW5K06aRJ3KvDiql%2BnIDi494n0IHuNAA0Oaj2w%2Fg6G%2BIsZifjq7KtcTPdPNAE57Zwu1iAEoU%2FUgbcErHPht%2B7Y3%2Fvc%2FNxg2k401t%2B6c8Cvt8ndvvbjdZI0S1RdkCkXHuBdSJ09Tw7%2F4Nycq5e7mgWrQGn0qvA1h5vnWCu12CZNjKec1XutKiWiTqOEYulMcrkhv1ZXum%2FSy1wNv4Y5pElO9XUzxD0zEQJJO74grabgfKZK%2FdSKKi5A%2FGjx56x0xOUydGpRnxsqC34Qkr1JvnKCdyumyKhMOs55V6cagniPlnaPPjzGdpyZq2z%2FrWlHP7htO0%2BOxXmjwm%2BoMkjDRibDHBjqkAUA2Za3qcw%2BbXWopNMc81j3O%2BSxJdb%2FzY2YDvNfkWfJPiA%2BQWXJS6sZRD5j8B7SEjc9wI3QOzfmPpodYvGUHLPSAXxTlmzPtuPF121AHF52BgjNfNPuVqRq8WOgcxikQBxDVEL720xIfs7dbcqa7gI1d5b7r05ZYmmk68A8iX8psCcP78HQOfP6Yqjrtxr6YUZMmwReydfZj43Afr5mRhZ4IeSmY&X-Amz-Signature=5204990f7034a3fbeb50f003a6478a7660ed2ea8172af1b4b2049e33831887cc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

