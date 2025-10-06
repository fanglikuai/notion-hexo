---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R7ZHWWDL%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T180047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDq6hZFDkDEH4PhCqk4TEOBHMMfZgJ9PYBlCLkBNaRoogIhAPnzkpQOVg6dDIL4ADQxjqzVUziBju0mqtO7pPOT6AkxKogECJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyVSblxF9Cg5gA4aLAq3AMLWTVPcZfnKjVnXz6jxYKTVwabIShOWkIxfdTX8TxNjh42w3JhA%2BK3x5XBxeNbhfTS1%2BLozbKvk7d3mVWNs864HeRjwscNqVXkC%2FdWu5GBuAfPf2yly5f8vXQkWWZnWrz5kbr4%2BcIhePFQoRYV59QofzfxEZ5aLm2d0iu6URAIBJwzfB5hqbO7B59dMIgfZnTzT95kT9NaQftLVFg5CcxlKNeXyfX672EQIoVcWdBgo6FVw82JU5AiIHcNAf%2FLCNxhm3aU%2BAipJYFetxMviW3q8YEi04iJGZ7m0%2FG4Vu4BDCU8B0NrYJ0HE%2FaTw2wuO4mdhRa6kaR56JQLSZZf6daGS4aJAtFqXASSYi8jOwjGNgabYH%2FUmaI3EnB%2BIQKMl8lP%2BzH5stX3pT4JUEevyFG0Ek%2FvkgdX9i9lioHh7S8%2BATgRDKrHHfLXCtFjR1BEgh8J1%2F%2FPMZCCSKlu8vkg2HIemQ88SaeM%2BFR2BmA%2BtuiTPndBYcWeI545EFUgrUQYL5m%2BK0DGH5niubyC7vKrMIih8nexlYhyHEpea%2FysNE0Z2ENEWLNanIq56vlNwJHdb7tM8D%2F0hJS4lpO0BEawMNxaVYGDSg4IVhngB5QVs3B2d0BjUv%2BT0CNosTSxDzC5tY%2FHBjqkATB1ur2DxHtYFaUhSWTjJD4gjDkjVXRiVNyZC%2B9i4Owniqb8F0dklE%2BvDjoxgUD9HjABqoI1QkpOewH%2FDlwG4jTSIPfHwZ3hVIMQ458CM43tc4NHYiWiZYO2rYzvdd57PQy4OgjDn39IRAAIE24Kmq8iqqHXegePQYgamKDIxbvYKr7Ih3cg2pTkWZNiu%2FYW3mSbCSJ4dus%2FsWoFBJgg1w6VBQUr&X-Amz-Signature=2ad0a167e6932545e9f0d28d3efcd093cb894fb410a4fd0f508e426b977a86de&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

