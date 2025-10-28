---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TOA6Q25O%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEOVj1X%2FlsOeDS%2FklBd9JPDmjF%2Fuuw%2BDOhUAbEqVUv5pAiEAjI4ILFoPd9r8eO2uLbB4JBNDVfkEnpjQCuEaH4MEv1gqiAQIs%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFopVwTYsjYv5nU78CrcA4zytWFEs%2FNM8F4hkx%2BnnH6QqzOes8dZRwJRU7WQuQZWDK1CPCD4avHOPbMOJ3zlunjOoicav8UXCjguPs89TBEbt0wVuxgOmYygCit9U9rwrLEM7kB1Fnd90C%2BPyd%2Bsmum2HUJ%2BDErb4kiqvtolRNjfMKkWPd2LwdYOrc4Zu9AyrOs8MU5GNxJl4EyhNs8APOgGflGR6HcUEauUqdEFv%2FAJZiFL6fFSUEzIuQVhpdtlChgSB3W3RkxSdS9aLOPRG3ZXteqX2%2BKXg4JUqEtolNOXMgEnVEzcLn1k%2F9AG46NtN2LcYy6ics8GVTyy%2Fv74LDLbHfCaHt7nMh%2FPJQ%2FgeQp9PL4LPi%2FjI6pE2nwVfUxTC6A5RkXI0asLkyvf%2FlNH4bRscNZSIAtuY236xuWHcOQCVuoatktOKfZVpBeX5xx2lSD%2FwIPokOo189ONEsVywG81HQMMZxZYz1toORyyAiG5HU5WzzLdYOX16hPF826zH6YLDsvsPFvs%2BY9h3hajmGiaytF32kKutLHD55EN2BAMOdRD1VHwQ6HU2aSQJ74Cati%2FfNjn6vF6eLKap%2FweohoGVE22vYxlfRs5RcWzQ9%2FuweSPgirfcMUvMsFqmw3GUZwI6psbNMCYwE0wMJC9gMgGOqUBiZYFhZ3wGXiCkhRnZzE1GVzESguhcPOVDZB47AW37wZ%2BwvMpxuAM5DD3DDDhEUGKrwnF%2FCxB5q3w5cQPpPl4I4p2vCJ97dWixmBdNq6u3DzOy8h4ozBSfaUNWdeq0yCyN4WbB5FJdeEWb%2Bhsj7gfIZjQByfwRNEibFVs2Cl1PQFLJjg3csccTa16X7yQcKf%2F18fpqTZiRQ9c%2BTg6%2B2Lsk%2BC457Ko&X-Amz-Signature=e2754de45fb657fff3f36a2e7707a989a53c77c3a86a06b48541f2aac8a2ff7e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

