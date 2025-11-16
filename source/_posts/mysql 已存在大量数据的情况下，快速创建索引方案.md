---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S3ZFIV26%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T070055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAGknMPK78XRXByOrVcp6QFUo9L99jzOqKual0QNMELJAiEA7Vu8Qh%2FKHrBfBxS%2FErn1yKsatVVf43vQg7ErDJxbOLMqiAQIkP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGwUa9nkTQNtcEGe4ircA3iOJ5tTqZDKp832EnDF8ohM%2Bv6a02B3O%2F6Ki5dzmPfGcA6Sq3T8cSsq4VrzTEuYrnmxb2dZZHFdbnVedVbd4aPk8t6dD%2Fy67%2FHRTep6T54nLZsIF0qNSVhCSAUuiQ8tB31RH4ZVNifTTAb4IntvMVpMpnTbtGQK3w4bk1ZFa68ATgMC03sJSLX%2FMrj0f7ta1HGVTZ2aXqq37oEG8JYMoUJ2BpJp4rr0ImV4dFNkgjMUeY4XwvsMIhGu%2FpfXDsRaBsvwKpMJgxaGpjtOr7u2ZQpPS3YjQ69OiiT0oqnmp1RdiD46vrZD1eAk7Ej4Ylz%2Bw%2BY2TTr5EST9CbAdxSxKwMjdytx%2BIZYZ2ksF8QUXGPNgZiR2xGdffbew6YbzYEOvcWA3%2FeLyW6Y2hqrHa2Yx3YKcXSrKUyC95NAd4Rx%2FPjmaM%2FUvMHYPImHGiu7AlxyDmqmROM9wQFd3aCndOpcsJ1TtSguTyqu1Muyk%2Bxj%2BdINeBRZF56XTtAJHHCyBzxiR9eNkL8DUeOfFRx8wJBhbFumbNqWCfyO2jLyPjEM2WXfk9yB9zLRH4M5mNgSnqsKE14AGiZVC1lvdCrJiFElyFrEi9EEagWivYdOkCIgD%2F0j%2FuN542qiJncBVuNyvMPbg5cgGOqUBH2gHBCNwHlJ5SxrYo%2FE0vBv7jBxAlRTYONC23wr7AflpW8HwjxyCKbb5MrlQLJGoscA5l2xJ7Srgkf8YJgr0AdO7gP03QxBZgcvsrpE3adHH3KzP6XPbdDqao6KK8BYf5jwtjYWCMKeWwg1ZS1Sgi8XXrNHMMCY9%2F4G4xNCfvbmgdUROtsu46xHXzHNDd88OcX3VO5kP4j7afbMCtyFSJiRDHte7&X-Amz-Signature=4208008b41d65c4511faaed198bde3f068e150124a27c89599ee5a5b9387816b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

