---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XX3SHS5J%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T000043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIQCQbiRr33xUOySsyirHcossQOhHY%2Fvk%2FxIBPr4PTpc9eQIgI3LsWRlqK3u7pndKTwrRcpAnhQAQcpppaghqAIyjUhgq%2FwMISBAAGgw2Mzc0MjMxODM4MDUiDGaDToC9qBMgjBFolircA3PRCDZP7Rz970NZrLuGu%2FezNtKHXt5FVyKX%2BIiahyKtF4pm2hh2ylkDgoGkE3pj3OAaljCojy%2BeUyGpMU04obwr%2BeJqbzWU1MAn%2BTVs%2FbzNhlLlkSHwuMNKoX9nnhRZUD09LR%2BufY2G2yimWXLBTnEAuyE%2B4qFk%2B1CjGoij0drlIjub7vh93uxouu81oU6NJaaBPmXeOBcu7Av3hUQm174Q847JaN%2BPXXxnHTMavI3mFYpEc329Wn4Jz49NxWak9gZ5%2BWRWL8lYXOblIvSpWzSBS3%2B964py9drPe9WPoUjAqBPStYLDWFjusFA6I7XGMJfxFomYL2Aer0uI%2Fmmuan3xPrlS3FAEh4PWLBdFD%2BV47YM2w1OvUCyJ%2FkoLj%2FjJKZMXLFwU5bnF4uyqJS5MJ3gck16hI4TkktKE4jeZd1%2F7oHVYJRkQVL0Dcukfu2Q8D8OxW9pfiGN6tqxg79MsBbtH0cT1zAu6gGuNRJwWNuRtEHeztOmRpH1ad3fHZ2gRpq1vA8HZ6KVUHy%2BZM%2BtZbnlhN3M0AOa4qIxAtTglhSG9KoyiXPc43F5itPSBi%2FpzcmjR%2Bt4nVbczwowH4PBVcJ1ghDJKeGJcviX4q7PNpJbtseDAJFNuTWmk00GeMNmrjskGOqUBj9d6WzRc0PTWPGesXPpz3I9%2BXEqzN%2FNLBttrEz2VIWzCqWcYv%2BfuFDitDQOVliVT85a9o8sOLhb%2BjcxwEYQya8o4levKnXdiAfL69ZGaKNqC0YvE8r1fcICSkLc7uTw%2BHaS8LnWKY5UZzHjOYMwplcBb6atY8XYRndPoVydPTzZatX5jvhPwuygsZg%2BMU1%2Bej0llr3aLtlwlKI9J0ck8SvOaYtmU&X-Amz-Signature=61c12493b588303de8d3b622bcac2906b95b0c8e3166a103a6bb572a1df4b780&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

