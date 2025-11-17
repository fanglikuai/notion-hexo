---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VWBVD5OB%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T140055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBfVoPJ3QVeQ5cLv81d1cT2cYgLrVD9I%2BoKGYEtSTMWNAiEAvJ3B%2FH262brw1mLPy2Uz84%2FJR9QqWPiEdFkO1QsM0aEqiAQIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDECaz5Upay1jv8MnGyrcA%2BDXmzFXC8NuCSFavyvahkaJhvHltB0a2yEQqqNwFgMxrE%2F3b6DD6xW9PmlE5KPfiaJC%2BQP%2BIlTiPuW%2FcLLxs7I7O9WMAWLRTuU17fBsAuVH%2Fz2WwQWROs26Bj%2FCQ0XqwUJLFyliL7Nskp%2FLL00HfUYmSVf3YFiNxb0V304F02DM3EMXA%2BZcWKUbq6uyzk4Pov8iYFNqnT9O9hBsdsAfOFqgr2zeKLNPxWMyAMHt1df0DB8Q5UT6fKzTH0UeVpe1F%2FSVYknllmJJQDRAzT%2F7%2BAsDItm3gH8cJJQZgURMxKk7KD1fs8Dk5G%2B7h2cUnvfSTSsapolYo25bAiVAWZusvm%2Fbs9qTnmO9HwwiAYxnMtnNeVUj3KbC45deEXWqoojhUAgSrDOE997LFjV%2FvYEa%2BUp7gRTLbnYX7O0IUEkWMVMcHqxVxqTwNwKTn76QJWDyJIWhfck4K1lIzjmJ9GeGcjclo7BPCdsRjPjSqFmXOu65n9m29pCIcc2IDyH1%2Fpe%2FI%2FNwJK6cKATzTQfpbtPqAR62MUpCXMUqyGVOQiiaEU5xqU7ATxzoSl%2B5%2B5nhKY%2BK7PMZa5z5uA5h1l72%2F30BqLxtEvK%2FUonjnJzyvTmDLBzMImMCVbhamNVhCXpYMOrJ7MgGOqUBRghCzS9OjpqRDBkIW%2Fcfz3cicSnvIv034QO44b1ABrYqX%2B9003PgYptGRSJ2gxcp3fzENzf5ByaGDn%2Fjc4xz6B%2FraVSyDYN8%2BgHN3Ug%2FABUIDrnP0CmUmZXIjb740droiDKlOg591s4ND0QbbobclAIPq%2BoqZgWkBnT2evga0abHL4v0LWDn0PqWqT03MV%2BBiVn4hmmf4HWV4qGNcj%2Bpshj%2F2Wbq&X-Amz-Signature=acbbd821bebc90f4578daa72fe6f91da9162ebfde7ba7e94ccba429048728150&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

