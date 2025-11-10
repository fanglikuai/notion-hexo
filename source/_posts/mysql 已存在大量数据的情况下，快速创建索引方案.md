---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YP6KTGNK%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T060051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJHMEUCIQDiLY66IWlisOWeT4QbbJHingk3eoJr1EZBfbxLzip9wQIgLZ0UnHo%2BDvdi3EChGS86Oj%2FnhvTwUTW5hUJeCBHThpYqiAQI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDut71d9KhxvhTKsRCrcA9Ty5ZHJ5AyOIpk3k8tjezu3OZCaY1KQqc9NgwJCeJmlGZm9Q7qFc%2Fje8zzY%2FNqvzSBvxdNjAmrLQYmQjHa7SxXZUqrX3oVzEO9JMF%2FqFBxqGRrybhGBALSukexQxq%2FCTZGiBhqLFoHaFxIiWXLJHjZdBORF7vd%2FHmg68341C2ol%2FGHttsaH7X9nbUZHGl5CBEhZ%2Bi1bxtA2sNwoKPDzbIoJzlNdDLzDZMxPuH6NzHjxmT1EfMs4bzrC5DZAnW1v8jVsahWjjcyflArBWJHLVWch89TuU%2BGXH4vN6UDp0WWKWeSQJhIA%2BL8zxS4Pm4yUGslukhlH1JCOaoeWLpIGopWN1zilU%2FU67K6xEveLRR0GHZ6U8rD4pIPw2lqnkBExJvaImBQtdMCMkhCWj01g4noKroWZOE7MHRrmJpE9xUGoD2acWgIzv3%2BifTtg1yfSEEWvx%2Fsqe7EcFCUusPJuD5xW5WB7RMcxZxbk5et%2F59uwNYNPUYF70Q4TpBuCJIOnGIBdO0MfjyE9Gb9QHxNHBFcPHri6ODLBJ%2BKVxN2mwl07zUWhp12NmV8ef41%2FM4MYb0%2FZdNhcchynsUmY%2Ff8b9dS0fSxi%2FvssNjEsuFWxVzH1ISovBKOgxbIbQGdFMPX2xcgGOqUB88TT5CgcIu4sUGa9cu1YBL68udJ%2FNzLzTuthJoztgMjvE4GqrB%2FnuDXbOMxaaI3xwXQkDbif354v1VdTFrgT2n1JF4zFKNiMDpeYzFBiICPpr5OE3eI3yGYX3tMxpvqCwTQl0uWMfuop%2B%2FbnUN7lTqvb0iUPZO0wGcSQ0N7z9A3cSOPCNGkP07OLrmUpwMokUpWWMZWQan8ge5mjD8aZn1OWdNJ2&X-Amz-Signature=4628f3f92fb98f0662e5789b3b60c41614151806b6ba0a37be9d80d5cf993c76&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

