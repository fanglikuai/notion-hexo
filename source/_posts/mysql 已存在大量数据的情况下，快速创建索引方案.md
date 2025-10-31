---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V3CZYFVH%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T060055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEUaCXVzLXdlc3QtMiJHMEUCIQDhuynwHjY7bQ6GFwwy333G8zSTMzZrb%2FW%2FwUZVEzwsngIgcU4WTo7TooaZ%2FlJAKfD0WwHFZ%2BUq9Clx37R5b8A7PCEqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGSmHzM7IyKdcoCKsSrcA88s6jkSJDh%2FtTj92CVqO5p7untDopRV2s34IsWZWg7TW3SIIfx8jPuyFsI0%2BBeNAAm29v0l%2BUebr6TKMXmTfhG8tecrGMXT3Tq%2BUojhz1U1ybgys4HIfMq1uvpJujdeE62CYok3XAQTxD8UHImYLTiGRqCjDyEGdkO03oSyYqqKC3Ew0KCx5zKo%2BNrQEQaoaqx5B6c4kAdY5vH%2FOzXK2A%2B0HWve7Pmgm4KoWhGUSQ38%2Bl%2BsaNc7RR7%2BQjkoPN5TYZgMoZuiPTpp3A6H0DBZqc%2F4RNozESqi0mgjFaXQDt5Eda6wztc9Rb3kcqhsambaArJBxivKAUKCLmdUD0AHCgVKGA0Zd7E9sA9sBntzdXJoNwCzMydgKdDBBHeLktRiAWPTakS%2BuVRbrFoWMCPyJDFW8MU0s%2FhhWYkRqzqpa9DhHvbVltUFzXJ2%2BcJKn92QWW%2BYBpHFebORg8sSOkRQ6KMwnRK4CE8rcMhS5veXjGASMoVwmqSrmuYMfSEqeSHi%2BM6xfbsGALwd6AS1Yc66YZAgpoVNDZItELiYUcaGaB0U8mIO0%2BUdw0Tquil5ruBKWG%2BB003PDSqcGFneX%2B2ByMrBKY%2BeYqzFVIE2Er7iU%2FmC9YrdxLzXAodXLJQfMOqLkcgGOqUB5dCJZFWmBXApG0t%2B%2FmrW1EH%2B%2FnflY33CjSiv7UBq9%2Bf72em1WZjdXgMKlDtdQKP9t2w1nLdTLfvQO1KiSFxO%2BiQ7ZHXP6Wnp9f4IzO7ANjSM6ocniEKbJWV6GPooL0cv6hZmxvLc0qlXFdlbi7uXOw2fgg69RL7fB3W52UQZHm914C5x8ufweXWA7pPqFW%2FjhI8o7I31nkaopacVs5Ah%2Bjp%2FEjis&X-Amz-Signature=7a6e3b5734c0eb08d282bcdc1a7b25f2d04f6d9daba5068397e262fb0df41ed6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

