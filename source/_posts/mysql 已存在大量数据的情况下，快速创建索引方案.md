---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TGKB4ISI%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T090044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJGMEQCIDAxWNiHJwQvWmpskfhdwSQLrOLim%2FxOOAv8luLMZo75AiAyZ8kQQJGPfxJzVAxg8beC4doWi7%2Bkv1F0WwGGj%2F1H5ir%2FAwgZEAAaDDYzNzQyMzE4MzgwNSIMi4ftxkzirSOH6LlBKtwDZhuqB5ARoh9y4e1SvbgJjBXHfU7Evjcf73Pr%2BBqaTRifrwAHxW7qyvprmS4Ctz3ogVUSQw4bdvYwjFk7g4fs96ejeDl%2FMLW5BcnZr%2FyLkW0emFTJB3ijb4xfzJdBueGSy5zqkbSX0B8J7Ls8dZV7jP55%2BrQw0BC0%2BcAWTdgExCMnQh9n0G%2FX0T1kZ%2F6a0WAubrSwA2ED7hSQxStpMRRzcCeD%2B%2FT%2BiCSkUCYsHDaNfsk0xYz5dRP54fDyFC70rrGB3mxNV42sxgIvsM5BUzIbKO4gN5WWHZ2CZaSHZSsHArCf6O6A%2BfjjOGYxS4M7sPc%2BYAY4CESVj4O0jC1CRZIGdpHRd7feGIgu10VLWXZWhfbosrj2ej%2BvQById29dDZPWlkFHS4nzuwR%2FE2KdfSny8HPjAtgTiPQ8Nob%2BXHNeDIRcMoUN%2BT%2FGM8EuanzCdHcHKo4i5JR%2F9%2BMRtJBbTkFewTXZNZnF%2BtPoW%2BPL3xIEV5WWbMU8yzzMuTBWD6nvx3339A4PgAS0vN442Uvrw2VEzR6p5eFfzCK8NMpVQXGQEVT7ZNVqlej3mNvddnxeNGYFtvvbWxjwxmLgI6Hw8dnNil2oZUx2oAZDWuM6KNatRFUIa8l8%2FfNL%2FLeA3ysw393LyAY6pgHTrG3zCPv7FQapdec1%2FpdIjczr6%2BqpGfedLUnPbtjEa88BYJV9qqA1xx7vfYhirHvlQHPRVM0xmCMameL1mapdnmIugCdCC22R0T5tp7miRJZz%2BUlIS1bPQ5ub%2Fs9F2LWr%2FRBmAVEFJVfjjUAYWAysWe37jOj28xaNnl%2F9l%2B9UluXI4ie6ZGDKE%2BZFHl%2B9YCn2Fi2SazhC7ZyfuTeuRpYQedRJeLcD&X-Amz-Signature=033f02454b8c295ef6f7ca1bc477a275204d42287d54c9843b85d18aba92c5e3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

