---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663R7KSK6O%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T210043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAT5BzFmt86l2nKB9rl36KlrU727EPez29agOkCLQ65lAiA6oY2KJtFltBjnkvl1BeZT62bVMecwa4Xot7MPFp2wJSqIBAit%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMYSbpQi9VROah1xolKtwD%2BWDYQnmRKE4FJLBcTBbXVQkePrs7eJiKqA%2F%2B8vm8pTiNr0EA7LM9V6fdQca4UDNZwpyBhsLGlElbOGD0Qzt%2BRlIJTAKNA1XjXFuWgWli4Ysk%2FkvISiUeXKOEHWeu%2BgRSR6FWc%2F%2F80%2BtWlPRUzS%2FUX1krCRxxIThHPf78xkjhrSyaZV58xkmhnVFteH2doIUTMAhMbieuwWobAw2JzodFht6SdXjljiFGNTFsccjNvZYXDKex9dS9P5raoYolIzRtdDwgREEEbZ0V0YH7NzhLIYT3kzWyvpLERnt6LWE8IP08othBKzFSh4cf6EKoHxXqakDtl0FNjluT1g%2FGp1yOuktzGUWaY%2BmUIKjWAuMmYjt11KvT%2FnrY4dqYMwkQvpyFAQTC5bS9BUGixjS5VjeD6Sw28WtJ4usOa6ylgjV3FNckMpD%2FMc0ZCUUvTobIZF8rEenEa2wVoqZfUB4%2FKLgMGtOHNoKU66MyOdrRXgEQKnQsCGS4e8BZ3OEWuFBdjWq4I0xiUpNa%2B6JPQ6XINFOPYB8uwNR84V5d1vVMq4nc60Dg7uC1ZthqtBaexXKc2sdmwj9klCtvz0uX690Y76%2FIDhhjl1W5MsD%2BWyOnDuDC%2FYytSFu7aaePub2XDJIwlYW0yAY6pgHZOh90EvvBpOmh%2F7dU94AuduNKgJOyie5jyidgN9NLl2iwTGHNxEx4A0t5BDhU%2FMhcoYNCYu7x5yleJ3IX93mxu9%2F%2FzfyMWllS6R0tudrI8FLfKvakNVMPbf%2FRdgoecxrIlXEpO1SD4Bcxvtxzj%2FTRJqKJ8mCORf%2Ft7YLPdRh%2FY2uBRR8%2B8Gs%2B%2FqnsVS9z4ejioTroY8eJ%2FiU5bGSsylb9hieW8H76&X-Amz-Signature=84e99e751b7e592b81254e25ad25f1846c0e4b53ca0e4943bdb53ffd272d8178&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

