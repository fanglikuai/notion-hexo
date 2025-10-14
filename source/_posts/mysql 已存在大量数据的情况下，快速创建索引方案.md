---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664ZRY7JRV%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T160056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICbKvsBr6to503fZI%2BZqX389N2TiWgzBa43I8V4UZJ25AiAUzC%2BEeh%2FJkf0l3WT85ITqgbs1peD7vb3OlB0WY%2BPTvir%2FAwhhEAAaDDYzNzQyMzE4MzgwNSIMfAtzrDO8QNXJZ9SyKtwDUKD2vE%2FzFAxbO3R5lAS1FLzkop%2BqdGKytEHsFzdqKTx9qb90S1ZJLRNBIzja7Tu0zxUYQ4NbhZ7m1xKaiY06BomLaBgfKMQxccggEmOKtM59Eqg%2FH89M5XDzoL1oFg6peazVMVgesubUVoJWpawMmodzNanOexO%2BM1iopfdbw%2FcpMiCQ3ZUbNuQtynqCFEHO2Ctn1dYnDCDHIdwOXtUvm0hCE%2BRSGt1LE5s0Bht2L4hS4E5Qnrg5HUySOJ5MR%2BZxg6NNN7CWBDUo1yF7sdYjtxPgTQ%2B7GxrsHSp%2B3GCEaSp6i7ETwxhCkSHF0t6xg%2Fe0uFabNDlxA2ItN9dYF6MSl61yJaudlalDjL3AEBtPe0%2BMvF6q1Go9QB%2FebyAcqjjxYWvNxLsFSI7JI0iUeCo%2BIjAvcFUEennZDa4YkrqVpaalJCy4I%2BKqKwIDf0MQBFfeN%2BUhSja7aaXBuzjv7n3CsI0w%2FR%2FOtNnXszIYuxWToJ6ezUm8TaL2bG8UNdvcMX%2ByNxlI1o%2B0t767lMxN29ZWH0yzln8iqaVt7X02sFi80v8872su9MIASKXqyIFSuIC1FK26s%2FIJKxi9foMBeHYEQnLdyYG1VkpNXFaxz%2BWf8Sj3wudxSijYmMl1mRgwptm5xwY6pgEa5Ly9LUrSIKlx3TD5wsZyBJGGgLtisdVukUr9OJRchS8Sg0jS4zdJXofmLsMBI2nHHbxUyo%2FNtFNYaqhW72O6sUIAd%2FdVA%2Fz9mzbb%2Fl4nQoCnXNHeTXz7N8rKM5rOjcLjR%2Bi2VjuBxBgijzW96OZrZynREZZrW%2BZuGhZZOmSTzyqbO6WLG7MO2GT5PQN0uaxfRh%2FKGkMuRJ6NsHNO%2Fp7%2F21IFftId&X-Amz-Signature=56207d463ef25498631fd5915ff5a3487dc774e21f91f153897d4d71273b1770&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

