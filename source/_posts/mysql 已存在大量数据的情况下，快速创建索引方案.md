---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667XDKPHLV%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T130057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCNBwofgP%2BxnSYF6EDd%2BXik%2Fhyj0IbpL7w8f6tHpdghyQIgANDD08IxOrcvwRkoH89njJn6kwQl2WN%2FQyegYLaHsUsqiAQIof%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDK9csSdt3AbJIgBKKyrcA%2F30MDKl5Ej3z80Q6MjzxwhCr4lZEz0%2BHmnoJ3cDF9LDt8ZlFe0xt9mEtB6zL00h7Sp3xm2%2ByZjk7SNnNpuwZkfDekthBhiNN2l22M6sv4ewZwEm9TuHf9x%2BHKzbOVndXxeJbgzzson14jr%2FG2x5l8%2BXgVE838M9CPH%2Ba4e9KvoS%2Bm3ryRh3pKm3EpCiTYRQV5e3iEqZ1XxnXZTUgmysWHpWKCTDqAv2mO5w4wuhbfEjKPyiXJrKH7aHnvvKWXUuaW0m1%2F0iozeC%2FF4fPtPm9YO3ITJIzLepjTHBVnpmeKuShH%2FxiLgimDeO%2F8koUyjQK4KDrYB%2BH2NkLK%2BpSQeYki7jkhAuAiSg5COQ7kmeazmSlob5ok1rs9DmSxvaOOzFMEKTUyy4ae9VCrnVI7oQ62HLAwL5sO%2BIFJKtAkxir9TjQ81xoQW2QACZDpN%2F8YZ9eq5bfiljJqxpFKNqqn50k0hSfnmUOfmRB2HSxHyXE86nDcg7TkssGl3N9Zq8FiusdHXNe3kOTCkXdsXJNK6rCR7KXvGdyUO6JEFQqqTmkyPxvnnH0IFRS8tB8XWmz%2FyLBNRoDJ9xO7YQIbiaFjFrj%2Fdc2tDGUt%2FI72oGOG3WueqLP0BzHKMiaD8BkKnQMN7nx8cGOqUBCravNtxo%2FqLdFkcBA9nhTpstRWVEu2d98yRwlIf6HBM9%2BTtn9i7ApB6QkuFdHyP%2B2RMGFgwwFqpVWvfW3e%2BRPCkVjWEBi0IGMs0tyAXO%2BzggIQ%2FB1oO1lvN%2FixInHvhwvimYbbvpHJOJJf5CZdbg9CpALjUnLDjDvcamLP6sQ%2BwyoTx3p%2F0DFoS99zn9udNTliX0ZwPp2i47FQyx2WSW6qsX%2Fzir&X-Amz-Signature=116a1e1e60c3af5bbcd2bf9c76dbc1c33bf3550ece30732db421af1aa88e54b2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

