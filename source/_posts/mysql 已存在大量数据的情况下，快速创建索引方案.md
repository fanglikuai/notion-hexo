---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XTXQIXOW%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T080055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIEljmx8e%2BCRu%2FasRqfasYyyq7Tu5HYMWo6WHVIz9HGU9AiEA%2Bd6cnsDQBpnMKzRk4YqGrn8Ap3BazSd%2BnKNF69pkys8qiAQI4%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDH7tj3adw1SofiChZyrcA2iYmxuguq%2BrIdohUWD5upAuUdXy7rQj7oXkU8RHVyYoTVf0%2FRGXMr7tPXGUfQJAabObFpMBEToF3jYXNAQ89O8Y0Tpt1YFQ3xhg4mit6viPjzfgYhjsTTxIa3rIY7Yj3HT8pQvbWWWF0we4poGcF5mdLduZxmH4acznWOnXXB2v3X2yBniV3bm3vOMJrqeKGkCYItIIFhbnjsevYP%2FNylUiuIqwk0mrOm9q2gpKE7Ig%2BdKnzKqI227%2FKEGe%2BIvOVWdZe0YPesLsZpFay6CPF%2FlrXpL87uIwggpj5cAjvbwmIRzURZF4HMZCg2w7cwxpKOJ7guODeegK0Icslk9cPAFpa9s8CBwTFqUMDp3%2FYUCa4VaFqZHtyg7Slb7g%2BosR%2BA9fGsDXJmYtNThdPJPs4Hsu%2BidMuYAtvOFwxshCCWZWJ6yYQShJfRN7RkkdNBTISb5mNk%2F2gYoaDuEUoQx46bOdvyrbdqGcqBl65bd%2B2iisoUoztRvluqLE6FtWLovscN3I%2BQ%2F1JdTUXHZ6iS84P0v6RUN1edAqKhTdQhEOrgY1EKo78IuEb%2BJAa%2BqVXmduQXkbBCbaQN479cxyCIftIrUbRh5OoV3RyqTOboC7v4W5sQ1qdRRTEvcJ13vpMMfuv8gGOqUBvlXytjhIn%2FgK0OGxNtF2WBv2BhC02hZjwdzDmivIIGBaccHhZZQjphHJmUWwgFmwbcKEeQbnyKx8N%2Ba%2FKkAjzuEnCB6K1kh92haGqps5NqCZgG3O8F6X3k%2FRa0w%2FwhvQxsCEWsX6ZWjRNmnrH%2BMdvcklXJTWlcHQIEYO7m7vhgIvhO%2BZiO6P%2B3s6QSBTs%2BA5XQl83ekYmdMvuS1rxzWv3L2LDmMi&X-Amz-Signature=08f748b440c3638e14a69653fd5e3dfd5999e1944db166274df84f40afffa5ed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

