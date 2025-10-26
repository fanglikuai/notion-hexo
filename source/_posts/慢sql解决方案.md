---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QYNCD4SL%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFkE%2BhANUdeB0dPQmDEu3OH%2BihyGtoDdmACW1h8F2%2FjVAiBIvCi0xVyfpeRMBpkUWXH%2F8P0t9sMshCZfZcu%2ByETv6iqIBAiD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM8ruB%2FLbT4CtpgXgTKtwDfmUbMyzdlWUWDs2p8UJ2nyhXZWjfRpvebhf86FRrD8p2ZU1gYm9QZLMFvtORtpxjTUEZc0iuiamre1wOWfImv5KMyXAcPOqNhrGizTM5KKR1dlmyZYPiDPbFxOXVhZn8cGOHsdZ5jINZ2LrhJ2it0VpKxgibOWjzl87HM038iFgvdae9d2NDI7TXDlZa3MhJ63VWItthnPTP%2BJjYDGDC4xMq0khGt3k4MDLTn8yydZj2exvCPfQPBMbtg0MqEp%2FBQK82calO2biwUgtcg9hhNHy4po5G3Gv81i%2BmeuJP3BXA%2Bi85Hw1ITyYBckCnQ3e9NfcXRnWlmJNXyZOFOVmm4UGg%2FQA6ZIguQh0IPIgb9RpF7m%2BZOUSx2VTbqR1I7BMuL5OpkfaTmTKnSaXJ%2BqmOjT9xtsn%2F0S4cwmaBL31y%2FqIEvMdI1PvtyBY2wSGZWnyx3dAlEiZPRHwJuXVSCuYBZP8hMU5%2FLiYsTd6OWEVzS4a9jdIRwxS3%2BUjugMPUPvUcoLmBv%2FlWsYUOCmDjX3xw0Re5AX3qDS2ZOG9xAcFOQhj4dbRoGfYb4zudPSmLNYEJrJZBhUtc9QBU3DNPjBjLH6AZqBTr%2Fo20zOISz9h5FGl3FyIhX4TyMcgLUaow6PD1xwY6pgGDcJeXBw%2Fy%2FAX7%2FWQABctscBxdi1ReslZDJVsIiuIBRerAIh8rQrhKdidwJDbmAeTJYsG0jjpGTuuEzh6Jp14XE%2BRXdmdXtyNAgI1JUfKf1NBDVZa99N6AESHQLOfRnk344diKrCpYekxXDpTXdJQwOKszCgX%2FieLY6u6Zewy762B951NoEO8exRIHn%2FWLxpAFfPc5dcth1wa6F%2FwojVD5FCr0ZxyW&X-Amz-Signature=65a84f2b0f6ae1253c09df7ccf15a5c469bb801fbe8e8c4ebeda6342e25d0b79&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

