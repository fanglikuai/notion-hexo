---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664ZNO7O3U%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T040047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJGMEQCIHdTaOjxas3bqBYGwRSqqAWBT7%2B0fIndVUNzVwyV5cV9AiBMqszgvWsP2k5cG6B1FDBIPqtMZIfqe2DyAPIcJ3ISBCqIBAjl%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMDFtol7%2FmxfUh52miKtwDcjmmkucJkjf9Q42pDL3rm9OeEkW4hGKJAhtucPHEOsDQfSSmJi22EEhKceJqda960vb4l6ef3o2SYwY%2FtxzHMBQolyNYKHzVXW1tl8k1v%2F33Yg6XuZ5cHWa77M1VamitVYQP3wyuVy2f07x5yl1Qth6r1XfuuebNguSSri1XHg4b9aOadsP%2BvhFl8LzMUgKqUnC5jH9i1CB4ZSsdX8LTgv1B5b%2FbSwdrztjcD5BH7FdHGGwJGofYzwDTTacPRz4%2BWJIYBsGM1p%2BknPd%2Bxj%2BEzROQnSpieiNYYRPmrCWf9gP9PPb2%2BWT8epYHmEUDeTxqWdJytw0YEZ6zajHpa7zyuQ0pYa0lHU6IRQBBrFzmRrWHU9kM2Qn1H0tFd8fZOX1IecdHYb93SN8V7E0KsAd7ft6f8xV76s0F1ecPwV3Y0MheGCU7sRLpSlcvjCUItt1cBL63V9%2F34n0GQN5KwnTxNZeRO4N3Ql37k1%2BSsH7e9CXi1D%2Fizv%2F8CFBltkMNHOEW9Omg428vZXXZ2a2j9X5NH%2FJJVWwKBTVPYxd62fNBAsZpkInrbJBXfP%2B%2FgLB37hCVePEZy5vuwD09CezfJEx5LoINey1xbiwzwCC%2FJfOM9mJbxwEfL1WxAMvHejEw6qftxgY6pgFHdlRbrJXeibe7c5p6JYdQMUQUnYYGc%2FAVyhVDsB8Fb4EDr1QSJbAlEMAgH1NeAowod1cxe7kMPeAhxD2q4O9wAZyr9WDdPZZdhhl0AQfaY8eAl82QR6ljZtkwV2mwAmW%2BkPt9esUZS81T4Cw%2B382KqW14bOA17GnLiY3kIo5LikP6nozpW4LS9gqhBxqo%2FleJPbbdcCj5AFWjdw5IBqewOXwN1ruX&X-Amz-Signature=2f946b563a42181f670f8ffcf4cb173bf5aa01fcd88181e5962778c0fba0e77a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

