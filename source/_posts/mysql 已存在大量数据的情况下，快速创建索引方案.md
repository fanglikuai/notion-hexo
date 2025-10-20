---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YTHIHCFJ%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T080506Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIBoMu5KVi%2BReXz2ogZePUrcbyI7OjfI1HH9Xnbe5TTh3AiEAlSp9AiJTp8NeZyRkEvjvCXviji8Zui7B5FWHrW9XsMQqhgQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEb7woku28VFbTdDNCraAwp8kUcYTJO0xWCCA1wq1G7I%2FC1Z7QV7YO9uLFE7kf8f3KRqPOcYvjl5PhenNQWiFpTidk0njSbj%2B%2F7QcwpC34O2U4vthsej4ZrMxSHAbTASIC%2F3aFGD50VZb4ehGkzI4KeHzp3E1rpw7iLVZ7CBYuGTvwoqBkWaSdQ3eIRaNgE63ubYxH3Vi5xckgn5HPAA%2F4RbOX82RCm3yzQ4cJ0tWG4mgetGote7Y%2FybQdJpqk8mcafDwhjdqTxWDYWFyoKp0JhiDlIHw3JPBwx4A3%2Fs%2BdNSNiGv1TkHtMEzA6VAKhVG4mQjuTRd60CyCW4JT4tylIXoKxHoocvuR7TWVAC1CE8HHMVwoS5rZu%2BePLLJdRY8CM0eQrqT9YgjsHh5rqokUbOgVv5lDIOuTofsOwyxt8Y%2B1U6gMdcxhLnA25k6dU4ThlDlgxfAwph4FDprXWfqn5SiG2Gx54SXNgFpzae5WhWy%2FA4IrB0uFzW%2FvjFbRYyWUNNk9j4Bs1gX00%2BMNDFj3j3UrKLoSuqWr4wNvY3xfnPC7YYzLaesZurtlH2CF%2FOwhqXlmBHiz1YixLK0dCY9KzBORlSaEa1fQJC1OBQUhmoc1Y16Jn48%2FkX4rsh6dcvcT2vHM9iX1fYy9DC%2Bz9fHBjqlAXH%2FrJhcDq%2BCt7ueJmb%2BRCwlSkIIevriDc69IBwj2Z6wxRh66A%2Bus%2BNG0W23S8qIMpY7EHDQ2nTRLPFGOAEJyaxN5CGuh1x0fBzy55%2F1g2%2BR%2Fl4UsnaNhTVm0kGFh385bN5N15sInriLDn3f9FLEofsyOESNU1gFNv5cwB2m9GnLBA1iYNlOPHrdeEd90cy2TDAesnnEYLkCM1FzA8IWgrL0r8Y7NQ%3D%3D&X-Amz-Signature=c50afe7770a12de6cbad040211e8368e62686d4ba7a1e53dd79a01b5b87046c0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

