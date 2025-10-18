---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQ7TZF2D%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJHMEUCIGaZ4eMYpEuA2tYT5vE6%2FqvePqIJwYMq9NHTTWz%2BP7sqAiEA3RVXVgAFejrmJ4SARc85nHGbd%2Fkdm%2BIs%2FyOOL3zN6uQqiAQIxP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB9zKrmTM31xmW6oMCrcA0SEW6ckyo3luzHZOmylXSW5QT%2B1F6K0RHMVrXEz314ISBtawNtuF6auipnvUnODfTmIKtg4MwNC6jo3E99iSyLR88zZVdhpt86BiEnAlV4rdXgRniCHqnE8bmX1JlL53RA49i1QhOqiXe5TmxU%2BBShNzGfdhoAlJnDVjBOnI5f09FmadUq50YYzn%2Buuo5kFV1t0Ea1FMHgpDK%2BRsj%2Fv1UXXR8CejTMGrsXF2ab%2B11lOjoL5fLwjuK9AQo0VKPwbhW0LJNpN3sub27hWsAGS7RYuCDTg7c%2FxY3rURGEZFOceitJWROH4LHLRFGnMc%2BzSGtY%2F9Ka6iRCC%2FNYmV4pr2qOGIGpal31XpcFVkrqty0OaCCdvUhwhD1XePGAAhzq0s2H1zT3h%2Fgp%2BXCEg4iaYWhuqi%2B%2FUz7I8o%2BM3Qr4Mk4o0XWcTwyYveIiHctRAV0L9CN2y1Ok0xVD4DHj3WSz52KF0Lj4ouj%2BI9vYnS8FLEFBZcxD22%2FYxv4c1wZIrm8j51HB%2FuYvPov8HlGqd5n8KTNXLpBdihWGmD2XrIwz%2Fy2Reu3ifBgR2WTcjicB4yYSluP3gvkzWJY%2Bd03liqqgIDdBeJEv2vcqCPwdzkAIWDOwXwcph2vWuoMRNSOYnMJvIz8cGOqUBar6grWsH8Dl3deEIV7qIkSmlznWLkbk7EZ9m1m65l9QsQ7a5N2HKrMaFzj6jDf1OpNdCA69yvpV0fmrmHqBctKG1JHrf7F5w1YmyyKjxPYxXXyqmZXN6dsoFeFS%2FZ83nFL3HigNomEeNjJHRv2TqKt42md4B9%2BXAaZkzvZX4fbMSreL7tQP2TiN0s4Uw8jFYZiUNTdR%2BaD1BDX%2FU4YwUcupL6wTi&X-Amz-Signature=ae528a28aaac509238dcb47ba630cdd5ad0367970cd7210d9795d224f0af1925&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

