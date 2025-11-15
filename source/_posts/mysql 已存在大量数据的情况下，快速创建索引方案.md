---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQVJVWKD%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T140045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD8hjbvTCPTvUquXZvr3PbRShZP%2BUAmazQRH%2BJYrpyZ7wIgQp8Szyp1ciry4kdeksLk0Z9tu5DNWWmop7QU%2BI64zwwq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDM5LvACgKcDEa5DefSrcAyx7GdFjoBeQAZIfnodh4dgPtezVF6NBiMeL5adzRSqeF%2FHnRzN%2FrPTnibixpRiFJzUym0BRsWcu4udKnjaQfdsn7mjEoV%2B0HmaD8JtnlV%2BR6dDf6l4jKzWCI60OWNcn5XSYirtu9I7M3cIzvNfTPZ5PStDPXHiBbeXZFbNNHqr2IkVbpEc%2FMCo44flNgUi1XpSDcmvDl16c3LXm%2BfNFGlPKdNk2zWBQH5nsxi4XQQFaA2seXBY9aUAJ6KZ0DOztRBz4E8VullMtqZJCQVuEilKdtk9tnwopJR7oWtMqn9F25tMELFaoiKf3qkhDCMCWdvkckXzB%2BrBtRjUbE1cb%2FobwweVql8VS6KP1EHahKOmLa%2BTKnJdyIbtbDEsROA9Jtb7WGl4djHanFJK3J%2BY1gSn%2BzCKn%2FzaGmruY3ecpV6%2BKiT9KrkX3RN2WkN3pfuJ8CwAH3hQ8pBOrPY4i8HmR2%2Baocqa7SQFguW%2BpRiwtfcDDeApF55%2BCavkORw5d9Xjlm8v0YeyNqTvmx5lPK9pTK0e8fz6ucHh0OnuyXsEjyuazu0c794Eos04%2F7w5u2J8mAXEWAgzCkAjh7YkM4%2BKFNCFMFeWNBo7NsGTzfc50FUUxPMafftOsL1KhOFeNMJaD4cgGOqUBDC6Zr%2BGJikoHCy9MOREx6gp0r8NL9IaHAyRv1PBJNA4NCszZSolH2ddttqsepQQNR9kQNxic6eTWFkSRDqJpyQrOk%2F9gLbGnd7uGk4Tm64pZhSsSES0wIwU7JdlLDSEEu5632vnKBCwN4PNsWaXoQfmJdbL6K32h%2FwKw8xO7KGli8tou4LfLyfkErnhn3gCdC1A5F2HYNN6eyZ%2F18ffsrlaSlOkM&X-Amz-Signature=36110e50dc54a9770d6345e45b33c88288e43a42d154cae257e6ab784c0be78b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

