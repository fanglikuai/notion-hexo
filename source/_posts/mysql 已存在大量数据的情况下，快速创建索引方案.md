---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QYQO4FBC%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T180050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJHMEUCIGE%2FrWg5reaNJ3zEZVbsJf%2BcKUqsYfJpiBW3rj5qZgmsAiEAsUOOTtly78DoM3a1d9UbV1o9Q4ZtoAjEQP1tQNmirqIqiAQIvf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDbeU991zjo2h2jFySrcAyM5cOBTwkHNZ%2BZ%2F9LOfPYxegOVvZGIJYF7mH9A8QYjeTRUSl5P%2F4v0sCb715oO9jn9SH3yyD4FIJVa4ir78RXzBSoeiuEUD01xk623haovRx0Fe48KsNvJR0k1vhSkxdb65yQBs17mecvcnm4C74lIEJedg3X0Mq5Ox6PZhnlKCxyx%2BmBpjc76nhR1Hyy%2FWznb3k3%2BRTLwAFFi%2FN99wn%2FOgQ9D7ugfOdfH8umQ7t1K2OQXbXX2x5eMZQsyQv7QOjq1pNSU2IyyXo6H03srkDh6uzW6ciqAvaT0tQth6dSna4tT1mjPvPxhpHYvM%2BvqaC59koyz4GrQATIeTWJMHfVJU5mkxu4TiE9EnCQh4Qci7rkJnKxWcZMLTpPPSgqXtbIZGISVUfxdJGUirGTPR5iEEHcJWTQ5IWbI%2BAdEWQLB4uWd1ggbviTmDI%2FATc%2FIXbSGGq50D%2BTYQahMf%2FBRjluzJOLf7YcHFvFSd2ZcqasmDVCcvWoPYelVWFiwhQGuC4Y6pf016ve%2BLF0HiLFoOAdGG0cUQAxFmLsUjHqVJ%2B9VPrLlp6ZWbZSj9HCZH8W%2B6e4zDKJpNCGV6Sxe5udN7Mork%2FlscE%2BuKknyT9A%2BX0J5Z1wwOIiN19R5t4rU1MNyIzscGOqUBRcz%2FJvFXJ4iyIQX0T%2F%2Bc%2BTuI%2BQjEKO5DddotKL%2BxPwxfrSSwip%2BSUNm75Z55XOQ1XA5rrejcFVPp6SIUE%2BFr7dMl7drGNJoUOR5AdUqjg3E2TVDc4VGV8kzR14ssMpRC0n0zOBPLWuxBm5pFeprIIaut1IUJw6LkubIPXB%2FiZXgo%2B3C7em7psbfVJhtCvP1OPsq1DDXBStEKXH8wjzM9p%2F9TiAma&X-Amz-Signature=a15dec297a3bbc3f48ed15794744f09ce7cc29a975530bfdf022469cdfa0a902&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

