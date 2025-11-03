---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W4LO2AQU%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC11qxrXiH656JwrCnMfxsP2%2F6zldpsOT7gDerruejScwIgNNWrWYFhG8pLEpFNiDZfTZ%2B%2BAqKqm8GsFMngCXDZWgAq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDMMAxqhLX4qqvzxo9CrcA5vzmRvPYkh9rsS5MGNKOrKLbW6c%2BKD33tjDETzNymbR6xKMKj1w%2FVnmnxokL1jxaKsb4zEEsP98XvufVTZjbPDfR%2B%2BxQ%2FlPqbh5xfbXxhDcxiE9sWSnVOdLINNolWRqrjeGC191uiczyHJKvCwLa4TSPg9yFAa3sOTd7VXP1s%2FmAURLd9cwBZFwAtLD5nAE0H7ALpKTMMAkS1f9iE7agNL42QrxDrvlGTBr9UrfwpTKbeSk15MUc82aHsZ6x8F%2Fq1Zr2%2FBdMIpdRwhBguqDDcAu91fTWHOT9HykGnoQ43TaKkgt6xDBVJjVKS7%2BtetIMdyPnhCb%2F0gohRnJyryG95JPXZam%2BeeosWLqSG8MfcaFd27jvFpiDvWI0ACjJwkOnYuiJDKwaWCi5BotfxDLX0R2b5IbUDunMGuQuB4Rsx62Y%2BCi0qhOyU9yU2Li2RCLKEKqOPvhE%2FfDWR0o4Sf90pCiXcVwDgHCmmUEafN0QOEbyJ%2BTQm560sO%2FuLrOiiCoL6%2Bq9WkLPQMxztbo4J%2FQx0yvRxy2N2%2F%2BLrKblk%2Fxh1U8I5U2DN23vaXVYw1%2FpRcacHGHhG03gY3p27fFlJsSmmoUIBpcx5fxJ%2F8J4VXN3dc63XL3J9GSlbHwnPy6MI%2FapMgGOqUBRdPTUS0MRwVRQgAJgAKjZdAcFFLsHp8ud3Qu2tJSLQcVyo28SukigjeK7bL%2BGalde2ASbH2XVxgZ4K%2FLCt7jZprYQQa7DNeF21cb9GF5OM0nGU71pkM8Ili9o%2F%2Fqz7RPmWC5Xva07XAZu5%2Bze1o0xWEoOUY2NeH8JTYaQhWWSgPVl07XMKBPj%2FYZcVZHv9xI70pOFhY4zogdHe%2BYDdg3lLO5nCtC&X-Amz-Signature=25c5a991978ed066973e0c3cb5487ebcd003bf45e26a487883cebdd91211a23d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

