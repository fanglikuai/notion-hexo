---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X3N6GOVQ%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T160045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJHMEUCIQCkd03GgzgF9wSebhtPB5%2Fjy9JsZkPYgm0idlXdrfz6NgIgNnNXZAGv37T4SyuaKs4ZSO5s9Lw%2FHmEU4jSgEeIt1joq%2FwMIERAAGgw2Mzc0MjMxODM4MDUiDGMKn4i5Xy0i30qatSrcA34owsHcX509GFLMUAHzckgggAUg%2FQmX8w2e2Hhtv6%2BQr%2Fp8yVZHZNGEVesbPR%2BkUDUJgRK7dzBwEiku6S127ubjuCoe8GV466Vd8IJu3wqt%2B0SByQlX1bYTkkX0FHBup%2BNWCfKdLiyz4RWXe9ojH8yfp2WJpQ6sfQdlZF0DiRP1JSF20qtUiXJu3Lme%2BgjDlNy7RTIKiE3nB8dPWJjUGraw%2FdrLXwdYSqGoXJmddHWoLpXUnw2XRW%2FZe8c75H2mpK7jgey7vZDmk4YxSBWkxssmHCYkUZTki%2BYVZy%2FX073LdkVn1Qc4WHMhK2lyaSnvqB4DoDRa0nTTyxyWTyPhFQzvgqk8qoX5uMuAhgdbXBUsd99XbB%2FMBggzEoVVhBaPiVjSZJi9t74DJ9IRJ6o3RNAg9IUukDovHIAPh3QHLiP7bgnPHL%2FU6u3ywuLanUMkQkQ%2B7Ke5OKhUQ4YbdoPcsytNkhsyCWGgUoduPzHBT24emfJXHEa34C4iU%2FTD3z5FH2uiEJa2wq3JB2%2F4syV9z8PB4cgUFThONYW2JeBenRe6w9oJ6yALmw3ebKa48mxqhOz1Rjkxi%2Bdbu4ZmXOelr7amusgDGitR%2F139ydJ7HHnHz8dl4iLNZ9bO4xunMNmKgskGOqUBUpJU7rfyhRqSD0IrM0cduLjb7Hmn6EFVLQSioor%2BKn3ITAyOPy9Pf%2FM5dVUvQXP6o1mfB4QoUUXcFbMhMR9dWqskWMGBRn26p7z%2Btk%2B2svXlMHcsxFiaQnHG7bfoBa3WmOZWP%2ByM%2FM%2FY9sVpqR92rVVUIHg9o30OwX2TcD4a9lU2ntdXm9YtxwnGFUHMRxsUtY5aXoMsocJ%2BR1fcghxMH6idEpB%2F&X-Amz-Signature=7107302877b2bb296a7f3d5297954788e9864b409dabe6e877bf24d93b2072a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

