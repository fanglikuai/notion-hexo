---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXGOUC4Y%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T090048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGeCU0n3Y6533HsA7T4JOwZjKbi%2F1xFbOr%2FUakpeRs6YAiB%2BbYXY3ydOlmxTy9vXf1HUuS7UMZuEJt0w%2F%2B8hfgRfsiqIBAih%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAyWJ8PynVPY2z2CEKtwDJVjlDnlGgbgDwxGPOmb56kvKGM7YHK%2BoDQ5CbTAeL0xqxrZj7QM%2FQ00pO8T%2FzzBLQGcdwyL4Uj2eTxH6Ilz8VN%2FudKl%2FXdXw1BD6FK6F%2Fu0%2BLjEsRXWqz5eaP0fyxxnBihgJqCPtIc6C4OlTh4DvcrPEE5y8mVqG8enTau6D7KgJAeKIVgygfUB35%2BkItcY2NW%2Btwia8hFlW1jAifmDXwK9kJjkLo%2B940VajujHx8tdGoViQlbQYniLmviutqx3QGfofN6bC%2BzK4Qvc1NNilfhXu1j4f8TBMuDoMexvPv7URe0lpSRazS2U8y%2BfObAbyyGv0ghn3lSGFk%2FlXkWReLdr7jUEaBbexAlC5Ho9pQQ7CNkNWcqUVOgZXbip5WDJq70uOUHXVOZPhtR94mOOykO2JwkcaVNPpFB%2FioFtJeHhHchDalKnvpNczwXBml4x9IRbf99TEAQ5Pys5Ojr7WedcdrKPMSBCWPvdlrvfvvbg8MxaPdsonoQjm0U54k71A9A%2Fqdk9CeUk1mLJ5CoToVbHYAcxzveRQFCHx4dlgt12Ertkyr1nZLTskZYpTRFIqOB8hFpG9OYPK%2BydvDePquObOlXbD9ba0DPEVa0sMTGd%2BxPDOaXPvFlFUPIswxNT8xwY6pgHcTIl2OMb4k1qeqF2XOorX4d2qJEqP16XwOP4GPlWxH7eQFiVIO95BgNLQly8ix0crddFxAXF1OTH5VB4YkysNFVuc8oparSFdRspB2OAZzXV8qz81tAMVrEGlayJ7O4%2Bn7LKqS4RRBQHM3TJzy9NRuR335ZfryTqeWS9%2FosVMvImQkqFD7pZFyzcRO2q721zhL7%2BFGIEvp8o%2Bk2KJFr7PRxUzMAmO&X-Amz-Signature=9f80be152a1a51198370188fe28a7243bd195c0532f9f6cd35dd57937a8f74d0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

