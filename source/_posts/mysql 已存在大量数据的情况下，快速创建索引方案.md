---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZTRZDVZ5%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T090043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBTL7FBuLDf3bcGMKvyGM2otF1UeVGR%2BqWypHFfBPfBbAiEAnTDD%2FfVjfGh2odY4MZzr%2FWQ5VVXb1Fe1IDp3V8fdTcwqiAQIov%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOAQReil%2B%2FMkKJtKJyrcA3WjuSVYV5LZdnp9GngiNfV3fxlq6A%2FGfNBRtpzBqO0%2FxO5icMmqq59qtNCZaesUFfdJJsdcbQEFoEsAKXasPWQaU50tK9SBSCfoqZupjaoZLukUK8vwGwX%2BiAXKlbl39b8bJ8mY6VGay%2BeUCO979X7gb5vKbTV9bO6b1lmWxss9P%2BFyojUPsqTiCFJs5RNsQ5eDMRJcJfM2tlljshls92vpBGXhjOyVAf69mD2NtSTGj70IziSSZdagQtAGOLEYcOPgFKYvb3kl7yLqo8FUse2%2Bu8itGdQszv0hIAHERx3w9L4TtcCnSu2DMBCcKCrlFFh9KhsPu%2FzOW4pD3%2F%2F5haYJdRo2vUakaaSH0bbEn1YoomOWMmvPkGZDheileZlls%2FMluTbxkPC%2FCKMi7DUXlOi%2FbEY8TVCouKC8UXJduoeULXrjFIZzSnz%2FNRL79VHMzN8nhxtRxe1C2HmNLME%2FVpYUj%2B0eS1f9z8qd3Ncr9t1rMPgJhwLbq7SLYWGbHMX4Bxx7yF6X8U00ejK%2FGjtp%2FJKFEMZsKepQM1Dz6zbGK3mHYd1RI7UK8QhQPTAVZ386o%2BNiC2PwQyQni%2B9X%2F6oEZ7cAK599aPIKWpfEO2I5oRGCqz3yIbRvTHHZbt8aMJLDscgGOqUBQce9vcsu7V8qBCa8J5s0yDd7tdBsK8HKvWaZwJBEpaMT2H1176XpER5DA4%2BqJpsxeuSkv9%2FQ7wcK%2BOD3t1PU7omp1eLQbIGmUfdz2FrEdjLUyDlUIM65hVf3dZxpx%2FBGer5tpx8lWHUICCpCxpDj0xGZKW%2FbV2pj2nD5rgHM2tQOcXZxtw9ZxzYAxc5CsA5L1nlePjQg6QIZNKyBriVOzxfea3Uj&X-Amz-Signature=26b49a0c5ee4dc39973eb185c0e506783a6665939a919e146f6c8b34ef7f62cf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

