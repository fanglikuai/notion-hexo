---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WBX6BL7M%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T210050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJGMEQCICV8doqNAUa5%2B02tWRIyCiHQsmkSdlq3s8e%2Bm1fLEosyAiBefzQ60Ug%2FCRCnR8JYPiSe5QHdEeEHbQQbOUFD4iCeHCqIBAje%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM9xYrXzn0E%2Fs1%2FRS%2FKtwDyRr2t3H%2FTjzPkrM47j4OJU3BDEetn8sPfGxuO0Q6St%2FyOIniO%2FhnQMcvnLwc5sjDf9Ix8bSkbOyVdStL5psGrOhhHiowq0o5XnTCoU%2FzGPzR0TPX7NS4RFW%2FcdLIYMKrsyMyJzX%2BBsvVB9MdpesdRovpt%2F8Ee6YLdDstBncOHqr3rFCpi2OvsBJoTvk0ewuksc4fFITl1KGJDh%2FVOgiAmiC4fUBnxg4Ds9YqJjvrpkUtIIWeQ2azqpff6ei94Djwe7MOSgd0opPt%2Fd3TpMpimXUXBWwxT4UEiE29c3nsbhsOUDatDaj9nQBQCT3av0JguxxnOkR2OCuxEdL40cnrSJAYIgah0xwWESl0m8sjG0Kh3jWM5tq0sDXyBjgfYzoHuztucQcVKfm1JKu%2BvdF3I%2BZCAp%2FIduSM3v3UuQaHJ6IsGFgS%2BZ2Bc%2FUkkJgYb5di%2F95TpIo%2BDlIG8iDeBqxbl4Xg%2FrJaPLsRFlxjxYxWcLglsAyMP17GAgGWVYhGu%2B88xrzy%2FA4xGhWs%2FGzIImzkEvl9sFU5OSmWbiSikhpRLjp%2Faat3yQc1T%2BL1egLEsXAQ2BwJW4RlhVYBAPe423sqobkZ3wQUhPSenUY5zi5CgKAgeYRFdOzMPO3k36ow9Ye3xgY6pgE893HzMLOzdUzVtlq4CZ4urEeyHEM97naEL%2FCVnOZbWqjvnthdlsz6PRcmsEWVXjKO6HzzaU%2FJNmwYmSQsShi0L2jPhQguU1n1LsB73rCvEGkoSLwDwEHtbtjEcBxrJ5HpQRoJ0IEHiMMZmhET5Ni6IorQqG1tqIVBeXrBeSkrYZXyoVZvhfqYg6QaC2cGvrteOQXiABkgcykBuvJBP%2BpCGTw23Isa&X-Amz-Signature=41cc0ac6fe387674edc9e5bd35a54a3b3ecb6a576277082eadd9300a5f76b40e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

