---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U6EB4WIL%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T120057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD4%2FrzRlrV2QXhFQaAnGuLskVwU4ou12XneYHsf0H%2Bu9gIhAJdp7rWT3a6uKoe5H5zfdG5wn2p5Qu3VrQm56lhiJd0MKogECK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxMkwBT%2BITA%2B1yESFoq3APk0SG4%2FxIlhKHALdATjEXR0RHPYilS6rilQF2uYRk%2B0ZWWMhB8AS1KKhPw7EyRNXqPvhXZy0YWpRx7IvbBsyOKVnnuXztea51y%2B3gBFvGaSbwnDa2WemCHlkm2GcQUoWaBa7eTOESLOQNogvIFPA6hAZZbQmC%2BvBhJGGyTkTng2PFKATuzA6C4rnwyeOIajmEsgRoNlWNL4uCkD4NuVgVHcV8GWchXWUR%2Bc82SyiDsfTtRsyGdsU5G5JLdUhNTk83ftBCgwGNqbt0EY0cgPRfd%2BlrnjmTMDw0sXG9mtSf6CrhnRmfr5yAOYN%2BdNa3w4xhIkxXpjvRrh%2BHtsEa%2FKwfY66rNlDXy6J2IIMN%2BLjP%2B%2B%2BJ%2FYR%2BYZrlpUKhvbiM7AJrEvxlVuFw3JE%2FQkDC0NgMu9AfNxjIkx%2B8E7XyA4dkubSQZO5UHF3BIj2zXdX5THDUnH1ZWctP%2FU57BjP1%2FEXeToBYhz4bgCJiM1yVwXStg4ac%2BP3MsS3gt1F7eRbjKt9MfYmY02jU%2BOjvCZxg8fFmAkfOiX%2BrYj8qhEp7MwhXogHSnlHY1mbG5CfM41KrKiWNv28QPeyQhd3N4easRV5kIWhNZZlDc8GWWWf5hRFqfzDlpzE699Xj1YeiIpjDbj%2BzIBjqkAXDYLmGHqTEbkr7vi4MBJxZhi3EHd1qaMPWRMULQFzUOJrDjDi9pGszzRNJxZUHijV8V4%2F5h%2FTp8bsULbtTCijwYW98tyObz5fmS6M3l8yVac23hao3QeHIkXDabM4MtjoNQok5QmRDSjMdnG0wjzr4sDcyx%2FufiWneIWTHP1Q2ap6GVUf1LYj8huVHQVcay0a4ZcmA9XlyTT6auS2pLOK1e6b87&X-Amz-Signature=12e8628d11b645447f0a55c7f4ee73cffb2e1f87e45318753380b73737628e9a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

