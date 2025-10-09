---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YJJM67SI%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIQD7R4T1Gy4Ng%2BBFdxibtQjzwhCJVIlKU8hCIK3dMAAT%2FgIgeJzQ7ptmZ4Qc0%2Ffiw4HM1J%2FhxJlRZ%2Bm%2F9YxrgghX3SgqiAQI3f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDbCgEVmda2faL4PdSrcA5RpQFQgWOopwbwij0D0l4y9WKhq%2Bg64ztzLPnZP1SZ9QkNUfdglpMsKgNi8llPKoIqbMVo8fmk%2FGQTtKvqbo6yeWtxVPSnQV9xk6IgxPFZgCt9MjMOYkIThTwGUtoJ4EOqlgAeSdNJmdElX79fd1GB0dhqLSDOlqJ6MN4kf5OU2kkitn0PMOxxn5uRub7m8F1aLxu4e9vKmf6mRI51xxcbw3WKhnVmWBCFU%2BxU943bmNqBDB%2BeYlUN%2BvMVg5wUcxhf2mObAkM0uuImbuSIqxosq40%2BO7tTMTNWFx4MrhAQVCNxxyA576z27uSFGMUVu2eqqHaB%2FaG8ZtnEwhBLHOpkdtApC7qmKnuUcAtrCGy6xEBisXg%2BOoecA48oE%2FBKB7m7pRtGYh%2Bpy1hdD3XXEItVz79CdEs%2FJA%2FPLxdWNe9tp3Ey0hkjwIA2dBXCy7WVH7ztomJus93T8K6jD8pgVt86ayzwf4VLYzIZT4IAmx8fyeID2PhlY7fgyA3e8UG5nVeE86LC9MIMhjsibCitCC3ZhhqR8AAK1REClAulT4oF%2FgakyijVaX%2FZFW1yUbwNwEv0sztUU1BpNbvanLH7Yw5YYUFlwt%2FsbrEOZog2%2F9%2FuKI6kdbtEbYK%2Ftkr%2BrMNWkoMcGOqUBL6Kdxdb5uSAJm4G7WH4ortm%2BdHFxG0Kj9BGpfu1rSZofVG68WePFe7f7bMyvJmOjFJlNBokT9RX2gAz8r%2BN1PxzYTj%2BEM9Ih%2B8ldHrlNXKSanpwIkMURHqJQfy%2Fs%2BDfbMcnmpEo0ux7wfO90hnyGod%2FSU1pM1glYxoJvsz7EC%2BLHBxDA8qprdT8Njl%2BOquCJaCo%2Bw3hdzI1M0u104vfKQk9Mg%2Fw8&X-Amz-Signature=553828f0befa4f3faa985c52f0d48340204f72840f78910cabe822063d1b1a88&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

