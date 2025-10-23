---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SF26PZXA%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T000048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH4OL3QL64xRQD4RZCqKZwq4V5jw8SxSvtLdcQhPmIkXAiAla%2Bz2wLcWQxuXYLZKB4Wg%2BtOKM0Jcok69ZQ2LUlxQdSr%2FAwg5EAAaDDYzNzQyMzE4MzgwNSIMOMBHd0txP%2FJAHX%2BcKtwD45owEDE%2B%2Bq5lHpSBZ3p7RCWHFKIqx%2FcH2Koq5xcZyjdEq4n76T9AGE714SBKg1iN3kmwWpntWPvw3YLq5oVdV98yx%2FStOGKaH6htwMCaunnQHl8Eq4La5V42JtazlPzha6jXebf4N9OdRp0tY094NG0JIbG%2Fl%2B5E2do3or%2B%2Fd%2FAX7TcuxVFfULPnCy6wcN9%2Fl2z%2B8zpQ%2F6vNvfwpvdoyhw1DRZF%2BqBWXcS2tbxOBck1BnsH0GiK9m3yOZPxGtCi8JqMqW8ZZeHnZCQCKB66Ddeq3jsQN6SlgNRFd8NRKSd9DE5WJQYCwUX5vcz9XAexUTKs8B56rs%2BULLhgnAPZa77mu57as5NRLk1yuDDCdoK%2F87AKZfSYBQv6BfHZNErMk8XBFGLmxo13D2rpXx7dkZmDMBCT4BKxYGm9LZO0pwOepoZmNa4QKkU4bZUG47wQYVe7AQK3ds4I7R377ujcKn7ISN3N7SYZUcVC0vie2VO71z%2BQRTrQjmaf0NV5euu49qo30fjp73cIV5uC5LY9wbKZytNr5aIiWtRpzsShXjoBpYou6%2BtWopNL1WD%2BUawXNlTUiNI%2BP3U%2Fec4NV5R36WAmtfOG%2Beqe%2B0lNo5gikAG6X3fiLgUEALZLKJREw5trlxwY6pgGGBWDf7IxBjmLic81Oqy7ZLIU8y%2BouYrG0ueg%2Fcjapwsk16aoHVKJUmWUSgk05VxBpy1gJPhKWxA%2F0xaoe4HICxYublamvXqNdNM8sFesh9Kp147AildDlrM6Cwe%2BNhBGb7Qc%2FnDB224RSyFIC8H7yTmfKzGQTQ%2FkLLQ7BGt6HfMoKHdns4dSqwpn%2FelL5MPGGyxVhVgrAg5XbgZbFkimviNnJ7%2FMh&X-Amz-Signature=3879e9853e283f7f0a207c10bdd0a5a4b8ec4505af3283f70a884dd5ffbe9374&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

