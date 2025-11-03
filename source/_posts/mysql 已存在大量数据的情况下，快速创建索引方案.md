---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UG7XOA6Z%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T080053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD66Y2SwFD5Pf7i3ITySyPq6u%2BxFWgvEg%2FHVfYrSvPddwIhANePD2UjiS7z%2FMtB3MEVlhW1XlM1wDeaa240EHmdpiNbKv8DCFgQABoMNjM3NDIzMTgzODA1IgyCGyLWaXMdt6YQcDAq3AP2Ngoh%2F78WC57jC3Ei%2FNYNTIF0QZ6nDcUSXwLuIe2e3waw%2Fhn1B27kGJW7yCiFXMXppxcN0Gw42150jYzHnIG4Fjj1WPMW4C6uxpp4CclUsEPJgMOetjoODQpccxdAG759DpTE7H%2FL%2BD6rhCKtgUt5a1GQz50BOb%2F0QNThb4EdcFqvlcwlpTjV3S%2FJqYCtUUDbjSlF2BeC6FMR9VH5d2QQwUtyucNS797u0X7GSOZnlVHLZmEvgBRwSUYi55Fzk%2BHq7jPNH1c0FMBhgFIrT%2FdhYc9bqW5RPoIQ6qWcNXC1yWCbPVdcm7BUehWsI5rBDqKzU1%2BpcKYx5H8D5r%2FpooKjscXLEAqZMKA3%2Fzgn%2FKuF%2B1gai1yXwDqguScYDCksVVVr7ITSebmn%2BEOVDDTfvWcIucoS1N9NMgAWEa4GqT%2FX1%2BZD8RA0iLzbIGvl8foaOiLc%2BMH3HUeNzunM4XnUtXx7HWoDkbSqG%2FxsS9qz8Cguj3Pzar%2F6Jxree5FMLkjs3Z6BLSG%2FD%2BokJEuZv29NmLAj1tcCuxUQ5XE7xyFRS3PG5TqUuyR29HIlKnhX3XQJO5znnFCdruXyJH%2BcMAacfXFlVrpYwAWTv7FlUX75Ny%2BG2liaesWHCSXAiCmG0zD4rqHIBjqkAeL7FlOiz331iEikSRj8JGJ9wa5HYysWP8MczDIBQW0BYcgZ1aDl69O2QmDaLiG2m60fNcg7pm%2FSYfS6tq1ng5j2CP2Ke4YODXuze1FBPCkZ7aOKztzwfchLj7NKTqlVBRxFnyfTmpKB4KGUvo45G5dZRYVU2iUyYiGjk0AzlC9H3zC7qDIV9K8CH37Ogg1l0XFyUrv613wCb9O%2FZ72vOpZUKYr2&X-Amz-Signature=a0988bb37ae2654acc2fd9a05a1b34ee14456f74fab29c3be55ae4c42f0ae513&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

