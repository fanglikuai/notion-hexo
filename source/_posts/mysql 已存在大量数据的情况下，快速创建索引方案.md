---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662BNLV7OE%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDUFQY%2BIn0k%2Fmjxuk2SKMjQFDYLM%2Bb9KRJ%2Bu56gxOwdcAIgHwC24J6g4VOp8aZIuIdDbfZmZmaopDumRoWv8TSOBj8qiAQIpP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLUtvWVOX1xCZjOrXyrcA%2FxqJRec5ih%2Fmxv5Fu5VmocCfTmmevh1WTjHywNm2ipClw%2BP3TSinTQCWIfJ3ZuB4FQ74koihpBFJDQDJ0EiCBOxjbhiSoOhTwNgVSLXMLbw4Hgk7GbFzHrpP1NR%2Bi3pz%2FllBcujA8X59zS1L5FOOW%2FOfAyA%2FWxrcPn%2FrRHxVpmeHPq4H5yTt4kd46WsmEQ5zyoWd9m2bpXAq%2B1OrS4IZCBEAyedoNyJnjhHbfKIJ7E2ipp7YispXd32DWY0%2FbZvhHUSNcXjN8PcObrvP6StSJkwNwkQOfTe8palnLRyA3Rjn5TjaCf3pYBhVVF9i%2Br1I%2FSLazMIwMP8rPmZnILhUozrBqdKc0zuh8JBNjDJ4Yy7N2CLNvbIp2vdbHAGXrOv%2Fj9xODpGLKAfzzmWDaA0K9VicBw1kQzKMxQtE2PkV4ipZE60aHeAbfq5r6sEAOSbaEd%2Fpki6ZMWfIIithX6Nix5MHuZ4rl%2FUtegYRZw7v7u2iELAOf7dwa6EvHrI1UeI2t2pTfmaUsXqI%2BTfoZey2jDRfLuZCfTLcTiUOQ0VOwzaj067Tvzm%2F%2FrIRaorKe6xmCTP7GySHpS0QqY9UHtUDORg5R2w0qaHNjOQ%2Fn0MY3xoIZSrJeeQ7b3soSrLMNmQ6sgGOqUBW5ZHiQ8a6MsLy3XLj0Ulvvk2psdKnG0g8MQG0v5L6y8gXY67lIc%2FkUP3h9HexusSnJp0VpjoLK%2BMeD8JSAjkl%2BOGc7%2FOH9pts2KegFQDwvq26ETvpupwFGOMwuVhSeczeIbQ0VhFHbfFTnIEl%2F3ZG%2BXckLczaiCh2ibF7m70B4F14HJIK841ZtKK577d0C1guoOTapU8800N8MfaC6wyscx5duxW&X-Amz-Signature=39b6fbac481522f395a4fa53a699df18404c48976a6c4259c313b3a8f9d5284a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

