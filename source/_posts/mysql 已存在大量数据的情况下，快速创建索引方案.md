---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UKRRCH33%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDLq6uiaffAbGph6V3zmxj1brYn8zf%2FKDxhX%2BDDM7BsWAiBVO34inyldSd2Gw3gRxJjw%2FxGGpXgyzrbqqXc9w80Tpyr%2FAwg8EAAaDDYzNzQyMzE4MzgwNSIMltOq9ejjf7iJJpDkKtwDAMw50kBxjh38PBP05a%2BRe%2F1zrhBquQdfPg7ibNQ5AcoWVgwbmtLLwnLTt2sywAFO84FcNl9IPtK19fwB5wAqH7Sq4bjA4NCyRS%2FT0OqxC%2BEbmGi9VGZfXZ4qloPsmKWxXn%2FV0sX%2FMboZLwB%2BUGRTaIVNcqPAcNfWWwLOe455CafQaekIBeO6yK589PehLoQqrm9kcc4bfb3cvcUaxd5NW8rRbcDn%2Bo3NBaMtyyy75lCZUOEEjshU7H4OQRFRqODJbXSlcmJqUlK%2B78wx8x%2BlWq%2B9F2OmnckB8kK695UiY%2Fq%2BzII76kOJQAgJQdIHJnAaGqNLUJLdZnyw4SGYZAXk%2FFgwTvCOW6FfpQbGp460aVfDR7XImPHh4KEHUABsAbMC%2BJWOz%2FGF5KHecKXxIMxhYzqIRc1tAEHJ%2FUTzkLniq9Y1vX38a5LOxZ1buqt8oUVLOoDbliisb9Snh4pqCFIBGX%2BPED555EzPkB2%2BUcP%2BGZXNzYXLcZ%2FVYlyItcVZ5vp26n5tT%2B5B83617%2Fnljot3cp9LawmNChviXqt1JKboqRUHNmFp6aOH3foHmQAvrFFuNrxLtdUNgz7XPlW3t1C0C6oP7Chss%2FI68pR3x%2F2U9Luyl2kIiAs774fKwjgwgtWxxwY6pgEO0lud%2BqHBpKSVP%2FBRzFnzSPD7YOMn7cK42M4tEfbJkoRQGNKIclGb6JQ6q5vwHyHCN5X%2FbZdvkqC4bKmM5IyYFsPn0NZryTnX6j1ulhr4MGCSQHfODRwblEitWO5xiby%2BADgo9XRmOiDp9cNrz0azDbkU9sZyhYF5IMxBBKvYknKzVGfi6V5kkgEdsDFmZda8DRX4BP6qUHdxBTOm2ATCwTOXlE1I&X-Amz-Signature=695966c61775d63f67f15026282e393019ee4a17e357205f00393615154a7831&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

