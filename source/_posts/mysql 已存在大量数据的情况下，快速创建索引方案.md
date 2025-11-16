---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T4YTHDUX%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T010041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD8nGYQQnzMNYYZUY3QoU4yd6vtZvw0vvPdc9XDn3ORUwIhAP2uDId%2Fu5OsIEszWVwE1kpWZx3S6doYLHCHcjvfgS3%2BKogECIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyphl9lkA1s0q%2FX0kwq3APoIBb%2BqnIvxI8Ohb1mRpOO6pC5SLs7limcQirt4SxJr2DE1kEDDhFt2hx4tcd7nFPQHlsjHnM2ahT6ChSlNoCzLUcq9tQHV6UrT5O%2F67AV0ST7gSWQVM871rEVqnMKywUJwFbBogdvvPWZ1550lR4YdBzhQ%2BlIuED3WHEWxiQL75AhBWrYPoymSeqFCeS1O8ch1j8ZE4vZ07VrHEBpEeUwM%2B0Vw%2FHOF1kS6mrFvc56OR5p8tmMRqCWP5HjrECqhoaBW2xFHgL6wnshT5WBUrhjfRij8vDdF7J4oKLMMcl9k%2Brun3pD7b8yQrGpURzwxCll3d5hWFOqQqwOc0z%2B6mhsZ1j%2BkXLMPVcIj%2BAJyXIjIztiYSlq1Uk%2BDJcIMQ%2FdY5y5ae9EduTr4JQ%2BodwHJwpK43sL5HwYbrhgW%2BGBivy28aJrv%2BVUDgv%2Bna4jadGfVSxWW7jZZrAzbMRshE%2BCa8sSPHq5hoPq7vzEKFxbUyAkQwkKFiI2g8irubNyMlFq6Hy%2BqBYWUyhYS71AsLbkb9hwnH1fwC2g1mXX85znvtDpSMy5V9mKgMULshZEK1RP4TgmYyWq5aOcMQhtWf8YQz8pFOG1Jqbzp0Xxx5MePaYVovtfyO4vmkA8%2BWbymDDuxOPIBjqkAZdOJKbRhWyzHQWGRQPHoboowzYG4QbLQn0Bhypj8N%2FXMQ4KAEQkUZjvqh%2FtCRMT0gW5fF58RIzlhP88oiGrAz8gpJr7IQF1HWrLCuqeBrSb47CW%2F742nq2KT%2FUDr8Thjh7Hf8PbHau28rP8lwcuzNFWCgZBeF5KVGUZaJh%2FER3yuFZ1cyVMikfh%2FyRr%2ByC4yapA6%2F6MW6tiBNrdAeYqitlxIqbD&X-Amz-Signature=6d83102fb20f14c68ce36b8f08a0e181f33845c347589efa8c81d453d18d5d58&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

