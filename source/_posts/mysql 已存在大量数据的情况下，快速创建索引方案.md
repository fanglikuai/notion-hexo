---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665OF3KFTI%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDOvfYfuVKNq8kG%2BPNKM87UG5mZQRoQBNDuk%2FZf7M1qPQIhAKFFak%2B5mzt0FTgKve%2F%2BpfOXdCaLYMvAj3ICNAvt7SOTKv8DCDcQABoMNjM3NDIzMTgzODA1IgzXzIRElZWZK%2FSBGhQq3AP5Itz3OwzBAXLItz5QbeY9HOb717%2FXs4%2BHgtpqEOsloW0yw6sRm7wyU4AIvId6Bg1PZuqCNBiH6jQKQG93wFFSCod1a3yvr9xuriu5Ksa2ym0792FNKFY%2FZgvtPoT%2BCeZjs47DIsv3ekCocmOI%2F5eB%2FlnuRWszYlDmgr27kUy33z5jWu3oHiITfPJHFh1ooLEr3NJalEKpUj7c0quDq3Gfxg1u5LFeL%2Ffu2mOG6amYdouP9n22QH3UXB1Br1u5zUnZEAIa4Z6%2F%2BzxO%2FsB1I1Zd9igRK%2FRMgyd2peZDP379%2BcwLHkbw05PN3kLyZLiDwe%2BQ%2FbP6ZmAOA8%2BVfChR9PdpvabwKrED7w1EYNY%2FRCoE2JIDgfHWj9rWO6sje6nIeULH%2FT0Y2STMtKa1HnvmqG2ozMiSWKWDo%2BNYUMphm3KQnoiWBq9OCDyVLM8JJw6crK3JVvSCfncCRhCeTA5AePCDRAQkQJhFq4VoIGra7GQvfoeGvho1zUEUTLSN86MdAzZPkdi4DGnsGAVIQ1vpBMzTHNu8lxICoSUz5okyWk3qvUkGTRhOnKjPCQaqThURAwG3YfLm%2FM9MJhRTrU%2BBh5lPiPKVdOVQB6OKwParHL7RFBnadsMT0DThm5ylrjDT2vvGBjqkAQXu85oU%2BtyYILAqfU7Sd5Ai4pC3%2BEa1IkwKGVqr5tNGqH43Mp8guSKQWHgVZBVd7f4XfC5wn7R6zSkyYKVsY9gNeOSlmWQN7GR4ldDFD2FFlC79FHuIT%2Bclmq2xE9OnBVT%2FCJBTPramRw5eMEgopQSqvUiuGoDvl9Td%2BjMMt3ipkETYYeNkMUWGxaUTb9Ku7BqMYza15dhmNbS1Cyu%2BYYXyq3Xw&X-Amz-Signature=d3e9f2366d603800e02af73b07cac10bb4e64acd53d498ee7f112f262bdac193&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

