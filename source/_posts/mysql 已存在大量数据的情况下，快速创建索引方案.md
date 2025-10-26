---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QYNCD4SL%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFkE%2BhANUdeB0dPQmDEu3OH%2BihyGtoDdmACW1h8F2%2FjVAiBIvCi0xVyfpeRMBpkUWXH%2F8P0t9sMshCZfZcu%2ByETv6iqIBAiD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM8ruB%2FLbT4CtpgXgTKtwDfmUbMyzdlWUWDs2p8UJ2nyhXZWjfRpvebhf86FRrD8p2ZU1gYm9QZLMFvtORtpxjTUEZc0iuiamre1wOWfImv5KMyXAcPOqNhrGizTM5KKR1dlmyZYPiDPbFxOXVhZn8cGOHsdZ5jINZ2LrhJ2it0VpKxgibOWjzl87HM038iFgvdae9d2NDI7TXDlZa3MhJ63VWItthnPTP%2BJjYDGDC4xMq0khGt3k4MDLTn8yydZj2exvCPfQPBMbtg0MqEp%2FBQK82calO2biwUgtcg9hhNHy4po5G3Gv81i%2BmeuJP3BXA%2Bi85Hw1ITyYBckCnQ3e9NfcXRnWlmJNXyZOFOVmm4UGg%2FQA6ZIguQh0IPIgb9RpF7m%2BZOUSx2VTbqR1I7BMuL5OpkfaTmTKnSaXJ%2BqmOjT9xtsn%2F0S4cwmaBL31y%2FqIEvMdI1PvtyBY2wSGZWnyx3dAlEiZPRHwJuXVSCuYBZP8hMU5%2FLiYsTd6OWEVzS4a9jdIRwxS3%2BUjugMPUPvUcoLmBv%2FlWsYUOCmDjX3xw0Re5AX3qDS2ZOG9xAcFOQhj4dbRoGfYb4zudPSmLNYEJrJZBhUtc9QBU3DNPjBjLH6AZqBTr%2Fo20zOISz9h5FGl3FyIhX4TyMcgLUaow6PD1xwY6pgGDcJeXBw%2Fy%2FAX7%2FWQABctscBxdi1ReslZDJVsIiuIBRerAIh8rQrhKdidwJDbmAeTJYsG0jjpGTuuEzh6Jp14XE%2BRXdmdXtyNAgI1JUfKf1NBDVZa99N6AESHQLOfRnk344diKrCpYekxXDpTXdJQwOKszCgX%2FieLY6u6Zewy762B951NoEO8exRIHn%2FWLxpAFfPc5dcth1wa6F%2FwojVD5FCr0ZxyW&X-Amz-Signature=0ef0195bd2f29fe880644b183de6cb92d1a1919c2178874e0f392ec7272f73fd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

