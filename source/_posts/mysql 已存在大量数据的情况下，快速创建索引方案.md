---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WWS7Y4V5%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGcaCXVzLXdlc3QtMiJHMEUCIQD%2BgBdTRo8nlRtZnCkl6x5L2HX%2BoqbPQqkDbgQpD9sGUwIgErgAlPfEqLLJKpgIlTbF9YUbThb9MhRH2ipgfLshKK0qiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCga%2BsN3hsZYFzEXFircA3rQwz4XnenZmhOrPfcDv1HyERaZY0AgKoANdA5wbV02FDAqXPfvCI%2FEKXIxxmfKqcAf0YojVXB3Ax7MKV0BljGaTdnhUbvuqDKqm4VMlvdg70KPULF8YKtYjzUL4TUd7ih6gDbPYSpmMOKi43LRAFOiRg4vesSM2Za5k77j9M7vVBNoV984roAVrvko86utwfz%2B%2FQR9s7F9ri35nA9QFY6NKBgBODd4yiEB8qo1PVCYSYg7kLV5Fpz%2B4ZFtWHMrBqzSzlCg9aElDntpIbKy7sP8lCklrG77VXUTSojbxENIYLKxoQCNr%2Brvn6eYr7fgT49mWzvZlhngYG7KQpCZ%2B8dGoa%2Fj0STZ5s1hZ5v9zjnatTvg65UfWcxoutWAworYQYZww23NfA3VijW%2FrV6z9XCUfjACRKLIDrctFC8kuE5JQZ9zGSHgfbSxgBhu23wBI6esx9ufnplzPMjroQJWR0%2F3sKkhN752s0Nn%2BTFTAA%2FFofpYoajlNPI1lZ6F%2BN47QyzoCKx7ok3EiJswKxYsgH%2FBZF4ZZvV%2FOGeOoCuR78a%2BYWJbyXnCytZNQzdsI%2BdOXeVv5VgvSDzNzSUeDY2frX3bbA0%2BpZormKHJ2P8JBjthPqE8g3zUDLOh6WmbMIrX78YGOqUBuxnuxoXrvbc0hXtSyzWmWUXHJ7O5ZDMlkVHQYUnl1nrp8qdg%2FZ9kK5PBHLhOPf3Vpm049H8hEDoX6zpSkWoakw1MTtb0fNdYHBy7hvhDjejCmWDqpbFJivkFgLrpHuSoWdf3hE7orivedKaueoP6bcJ%2Fx7zkD4VuD7tJJl16WFCmDMf35NJn1v1BYsX9tbag43dAWP%2B7Xidvamnq6%2Ba3UYJOjlxo&X-Amz-Signature=8db8f80e297a2b06fdca3dfd7a32c57e984612f463e3bbe8a29cfbd92b0754f0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

