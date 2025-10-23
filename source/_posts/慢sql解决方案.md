---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SF26PZXA%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T000048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH4OL3QL64xRQD4RZCqKZwq4V5jw8SxSvtLdcQhPmIkXAiAla%2Bz2wLcWQxuXYLZKB4Wg%2BtOKM0Jcok69ZQ2LUlxQdSr%2FAwg5EAAaDDYzNzQyMzE4MzgwNSIMOMBHd0txP%2FJAHX%2BcKtwD45owEDE%2B%2Bq5lHpSBZ3p7RCWHFKIqx%2FcH2Koq5xcZyjdEq4n76T9AGE714SBKg1iN3kmwWpntWPvw3YLq5oVdV98yx%2FStOGKaH6htwMCaunnQHl8Eq4La5V42JtazlPzha6jXebf4N9OdRp0tY094NG0JIbG%2Fl%2B5E2do3or%2B%2Fd%2FAX7TcuxVFfULPnCy6wcN9%2Fl2z%2B8zpQ%2F6vNvfwpvdoyhw1DRZF%2BqBWXcS2tbxOBck1BnsH0GiK9m3yOZPxGtCi8JqMqW8ZZeHnZCQCKB66Ddeq3jsQN6SlgNRFd8NRKSd9DE5WJQYCwUX5vcz9XAexUTKs8B56rs%2BULLhgnAPZa77mu57as5NRLk1yuDDCdoK%2F87AKZfSYBQv6BfHZNErMk8XBFGLmxo13D2rpXx7dkZmDMBCT4BKxYGm9LZO0pwOepoZmNa4QKkU4bZUG47wQYVe7AQK3ds4I7R377ujcKn7ISN3N7SYZUcVC0vie2VO71z%2BQRTrQjmaf0NV5euu49qo30fjp73cIV5uC5LY9wbKZytNr5aIiWtRpzsShXjoBpYou6%2BtWopNL1WD%2BUawXNlTUiNI%2BP3U%2Fec4NV5R36WAmtfOG%2Beqe%2B0lNo5gikAG6X3fiLgUEALZLKJREw5trlxwY6pgGGBWDf7IxBjmLic81Oqy7ZLIU8y%2BouYrG0ueg%2Fcjapwsk16aoHVKJUmWUSgk05VxBpy1gJPhKWxA%2F0xaoe4HICxYublamvXqNdNM8sFesh9Kp147AildDlrM6Cwe%2BNhBGb7Qc%2FnDB224RSyFIC8H7yTmfKzGQTQ%2FkLLQ7BGt6HfMoKHdns4dSqwpn%2FelL5MPGGyxVhVgrAg5XbgZbFkimviNnJ7%2FMh&X-Amz-Signature=db5ac2032be5e4073ff446fee103ae5454c12f6649069ca32c9b040333d8ce40&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

