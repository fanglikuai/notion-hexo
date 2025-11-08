---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TQLHMA4O%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T120119Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJGMEQCIE79TOYD9tXCEykQVLD2qhc4QujYoq%2Bt%2B2wxEGZhubiEAiAlo5FCtm5slE0yzFeqX39JWDIPJY4CB2LhASmQeRV3qiqIBAjS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMLIyAlJvnLn9fuZvVKtwDlCtB%2BqzbnjlaSnnsG49a47Sd4cpUqnlFj6gxUjRCyXJjNzfxKGDTlrxjkO7MWPtTxzYlUm9HQewu7WHPgsiovwZGMyk1rEymSOH2hKKTuxvtzESSJsvgAZWbu1AVB3nZvxwuC6YAWH5NNNh7yrkK2UjMBfp69NqS7agx9KjQgQyALWgRXL6pRJk8XNYwwz%2F7JCQavm%2BP3nU9yo8jumKsCA2kM10bm0bnQAM2LTHGo8kF9nk%2BgGWnAGmgaquFRgae9dHuQFYCjxtlQuQXRgdYAUttGwfX6Lnc18JxXuxcI39GaegdcCDIeLRg6eFJWA%2FB0etenlmlxm4aWPjOi%2FuNz6iNJjFotVT%2BqyfejleIeLggJHQL0T4skhgotRMo4a7a3iSR04MBPvRYUhw7eJglaKL8GKyQKOKQ%2BQ15iYnkAtS00AjO2q9EQWdNxtvNQi%2BVwt%2B1FA%2BJu2yDH6JfcljoNjGnNU6A3%2BIgdz%2FXRustiU1Oc9H025e1TjV%2B6RvyuapoHmx990T%2FGRTStRqGqC8qE2hnOMfWTzxWDhjF05TEcEyB%2FsZ9LIfOeLBs1YSty8bfOh4YiO%2BdFHX%2FEOYS4RAEXWNIJ8zemnbjA%2F81d3UxtOYPl%2BTlzFXwo39l1e0w5428yAY6pgFjse6Mvq6Bxg9wuGmhvQ8jjO6DIjs0f7vp4O1yy9Z%2F06iCPduY80%2F7C8oEOyRiJtz3AWIx5PwwhoNSX2H3r03%2F7c5bhvrXO7r0cCBeTZmXiFk5HHa%2BPwXCyN9j%2BMAi37OQBCh9MVCki1ChQycEEyyp%2Bpyq08qV4rXNkKvwHhcjPn%2BzEUIMli0%2BdjEzVarwmFs2sehoPwo2Syqy6Y4ud2f81s4nwtGs&X-Amz-Signature=3ce511cf8905fc6194cc0b8391950a1abea48dac787e341a3561446ff7b503e9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

