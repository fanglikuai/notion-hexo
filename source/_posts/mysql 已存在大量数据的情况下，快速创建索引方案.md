---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNDNGTPQ%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T180052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJHMEUCIGDKvD0g6R2kXfEB593FvjR8VLEYY4MOESLKybGHIPTDAiEA4yuezhqW%2BgITRA9PLcpmbF%2FHU0c%2FOmtBtXQgzLf9xYIqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDG7uzAvn5%2BxFPjiGoircA2S0fthibS2HA2AZUw8%2BbCk2%2BeIyXMumEG%2BrEo%2BAXR4wMQE8mKc%2Fo0j9QA2GR1dU0QmwpvJhSsUL4IrqHJOX1Rd96vOK%2Bl5uS%2B3ODs9RRDVh77La2J6uZBl%2FpxEuCPpKn1a2BkdNkbKpiz4ed7daJ53ZB%2Bcm4WbVlKAFw8s1Phu%2FJ2hyBkY%2F343qo%2B8Cd9FruUrl00OgW81SEdqmhaGMUS7OwDTphcjv13gI0LFVNMFepku2V%2BlmAd5uIY8SpRtNAvHcYfNJvxc7luviqdagsSUwAP37PqKBNS%2FV2nk7XBRQte91RhoYysr2eZ50lCnl5dlXjESKAvFG5kLESBWnfgy2hfO2YGEw0YG1VimPruO%2Fi0cre6%2FvCX2lVHPXYr15YbJNa8rCgCD5LgIQxNqvX7tcR3hF2R3l5dJ7en34G%2FMndI8uwxLsMl62r9WnrnKwbQSLsMH7wDI8Ud5ZWDxRhyrXPwreBt9jHZ5oeYbr9BH%2FTTBjHcR9z4hne4ZtjWs6Z8Qg1SpQgoV1TiUSdFwRW11iPDH7HBl5T%2Bc%2B0A0SweevAAYupiNVQYkZVrsC%2Bqzrgj%2Bx%2BWYQGaVqcpDt%2F3NGT47BbdILRe9NtQWnYQhksf7BhuJsXv%2FQRgEki70ZMO%2B22ccGOqUBSJyJo3fHb0wDhqW8OCfKhk7H50wf%2FPFzz3ndEGzae4%2BEbR7AkFdOo1vcHuJimeIfi49Zd7xffLerIbQEbRuHxjgz5vlpfm7ZcBDdT%2B9DCWTwPxY9tu0Zzv24TytNNhIs7kGxTqrl3mXxfuUzXxK0QAvSEaGvyVveCtMBTAjCNlTsy%2BQmToyS8I7XK%2BTrfeAAP1QAj%2FWFfEaJFDQttMWFfNvB5%2FbC&X-Amz-Signature=e3f8654acd56562b3ccf5ca547445df00b1dc7a374a8cc27485152d46847a7ce&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

