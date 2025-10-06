---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XOV26MXE%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T170041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIA0j5wp8aHoIjO54INl492w2EPuCkWu%2F6baM4iXreUH3AiBcxII1LFrxgOTo75%2FTwWKy35n9P%2B%2Fhi%2Blph9VfD5thMSqIBAiR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMNQfPYMfv%2Bj%2Bq0TKoKtwDAw7xGOK11FLixiZKqhuLPyC7eI7M1bs3I3qO3LQBQFnY1HjDGcFHFwQ5igKaNpVY%2FhHos0HhfyO5VH9FUa0ByWL9ZDSxdnSh3o%2FMqng1NoEvptVa0qgpZXF2dvjzVp%2Fu%2FHMRRNcyFUHr0gHdHFneWF6Hl0vc6ktXlte0wQTIxyQBAWYy2kCFqPt%2BUB%2FbZ%2FXMw36LDG%2BTKWmKJivBb8ta%2FtzNZyu93KYQe1stOMRMBygt2CAvlKNk501m46CRCyLPg9CxqjmqU3t5%2FhdnOAHoCMBThX1%2FOHwkobQ9zSdeE76LojutGJYoh83lqZpOS1LWVuNyUpwUrVZS%2B7qjO4toQf2A6dPi5gY%2FWccGpAvIWzt%2Fy4%2BlfM6HDuWivlxji1ex3gwT4Hp4t0jZlYerNr8Yedv3gzKqZx53KIDT0oGmAR3s3I9AX2UaPDwyPvnqbZv%2FPseeZ%2FgzAvXLOIACIBP3wuJQJCk1JNAgZqlgH3LlKUwjpynMALSlaDOZOK3qkif%2Fb0kN70B2Cjm2p8bfkz1fi6RxOH49sjWqbV%2B%2Fx%2BxX%2BTjquKbNIGCFnjLgl%2B%2FGA9zZ70jwu%2BSVSdu74IPo5hZNKtqHjGXv2XzSgSMtKWebvH%2FnWXjTrC%2FKoV8lS9Mw99CPxwY6pgGOIoLkNkOPXBkqc42WsrtaWsMEwILsrm70SVsmYW2KPYm7fsX9iFFlYkW3CErdVpZNJ0uSlIWfITeO2%2BjPMQNUErScAQaxeH9vNWzXYyBB8mclGJA3nU3rCUHaXym9lI3HigDr0WcU%2FfOU7SiEvxl%2Fl%2FdVRAodtxpMBWg308AWpwGZJF1n6AeVXVV4Vf68r%2BQkmtIw1phPrKaDNP44X798laQ2ymby&X-Amz-Signature=195bd2a4c55dd49e1e8dc0c7925fa928493985ee360be61b37977cfc382332c9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

