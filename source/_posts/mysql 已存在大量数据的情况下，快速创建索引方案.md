---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SCHIXZFZ%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T040052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICCgdDMmXewBN%2FnJOrPMjh4lVl%2FSYBcr7Sjy8RyOW1%2BaAiEA2Q5xSbMgYNEhCwXYUJcE2hR6yo7fzluX1IG6Ex%2FESRkq%2FwMIbRAAGgw2Mzc0MjMxODM4MDUiDCrkTzq9ZTC0LAb3RyrcA8y9YyQraSwEBUW7xM1%2F5CTrGQC4dl303%2BKQRC1CWf76GgjasutuAPrH0OYv3Yb2HdNCoqXWFUdvLkyb9mil0%2BTcnEvYzCYoY1YBBA8vyK8yzL0lkR5YcA6Sua0SsBhJ9i%2FzUvH9iqbFvjuBaAhp6H29%2FocoXAg5hKUlzJMd0YQRzRjyBmDc%2BhG2UAZiACmrrDj2bpnlrTfLksVziQrsCqUkFOJZZllyyCf61ZOZZ1%2FvfaiMxrIF%2BDYl6ylKzjnhMZyeaqTvC4fQkkuLeYJjOHirkLGwZg%2Fo0Kh3S1u0Aty0OhK5pSxfhCREYR22MtozakNm7Xvb2aYXyMDLoh8v2ulCKCK0sGEdYHWbL2lXkn6%2FBhtmFFl0gTgtFpzlA4kNFJ31WT%2B8TjJ5Ly2vjqXsdMZqSKhtJjuJA1EFriQeofi2y4CdiVU%2B6RS2RRiTCXemv%2FulCYqCBWIejzba%2BcgylhPyePrKrtSXYmLeQwjladMyU5JTgRghmFYV7yo2GC9CGw4M8UWCkhwDq%2BaAMXQFDZKM8uAC92ZTcjnDrP4n1lztE6009pyriVzVSermc74CP6M3aK6eSJv%2BMWKuISamGTo5u3RQ4lexPSpF7pdGPABhPQOpWwgfQwEcZg3LMKLlpcgGOqUB1OIv8a3HFz2cvcoxJofKZ1QYHJARkYS3%2F4uYA%2F5FEoyr28wJcE7Pk46rYaVYbl%2BkLx3ZlfvDcD8ZQw4HEuB6AJaQI4PWEOFvp13zwvslXUih2fbpiS0a5PFMA68LuS3nBQQYwqa%2Fti2et46U7Xh%2FyZ5qoNfkiVkj%2FUymqaBjyniFEWXMq3t%2BiXgUXcdehK999Quc8qUNHj8%2B%2BldMbhGPY1vrHMud&X-Amz-Signature=b42afc2a4309d4453f5f2f846733376db702f15ff469b4c606bb635689d62fe8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

