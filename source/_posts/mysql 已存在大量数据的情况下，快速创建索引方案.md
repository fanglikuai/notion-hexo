---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UHB6W5ZW%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T070056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJGMEQCIHLtbS0cVWiUzgUjXMvcVEGVOffvy%2FeUK%2BuuwiNbiT5zAiB4eR6P5UWtXbyKcbNfucGZBq%2FabeUm%2FH1%2BDReEQ2njZSr%2FAwgjEAAaDDYzNzQyMzE4MzgwNSIM1c8k27%2BCOShOITY1KtwDkf8U8XxVkiarqRWyyhdRsYF8OnXqFFQsZ0raKEmRZGI6z2b6bDVoqOf57ntT2nwHiqj72vq%2BxtQeu5rEJLOsQiTTMe%2Bh7zoZR2fNBYkc4ERijgsiBX4nXHny2cEtJFYlb8jRANYQq8nqvIEO6v8Is4fItbmxtmS51lHSic0yfvA1Rkw1pUo52A2AY0ZIwjOf%2F%2Fx9CANCWgeIIFVOgCsSFxVm1KvujHyKG3boacsrZOLAkjuY8g1SzZAqm0boCjO%2BtyLaTUWW7OkxYdHGUJshjnhqzt1Z0IcUgw8RNyGRG0kQpBRxF7PgEddzn5NWBXAiHS65CFJmpF7frELkYOtQ%2BrfKZiSm6GuAFcPsEe5liUp%2F4MZWGlvzzfaZQ%2FbIy88HP8L9TtLgUMkryHWqhY3HkuzU76SQY087qj4136y31dh7r4Xo6q2bS03YvDocC4neZGOqbUSxd8hkblRcpdYoxDDFX%2FC%2FfJdDJVIFsu5%2F%2Bhjye1yCgDajNHFefAhTSC4U6hmw4se%2F93ajhRaDqZHFo8rUTAR8sNj8ErwG9FbKAkSrZmttaK3AyXAsY7RAcHftD6BnIhrV%2BsELkCuZpSrzrXBEIDq5EtLuYbwukqggyvqVbgBd2nOQixKI23ww2engxwY6pgGzJCESSqXZxMnlzA7LLeJaGzouUPIwdNGBn24FBPgVHE5ylz0WlibmKoh%2BwoleGGuK%2FOkaeNeOqtbkNPNjFgmOyqwrYwBCJK%2BpPdoxf0u7KFptDBF90eQCCIgqbUTkDrGpfErvvI2m%2FPi6QUftcRZn7HafM%2FEAWqC1o%2BdiZVbQsSeHvMXln0QS3jGjZI5SrK0iUAYS%2BXc9Xbg%2BWdgEDcNTuudVUOBY&X-Amz-Signature=f2223f85c3f3075fce6f55fb54a491605daf6fa9cbee23a0b40b0efe44aaf286&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

