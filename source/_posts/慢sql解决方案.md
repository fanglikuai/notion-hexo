---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQ7TZF2D%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJHMEUCIGaZ4eMYpEuA2tYT5vE6%2FqvePqIJwYMq9NHTTWz%2BP7sqAiEA3RVXVgAFejrmJ4SARc85nHGbd%2Fkdm%2BIs%2FyOOL3zN6uQqiAQIxP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB9zKrmTM31xmW6oMCrcA0SEW6ckyo3luzHZOmylXSW5QT%2B1F6K0RHMVrXEz314ISBtawNtuF6auipnvUnODfTmIKtg4MwNC6jo3E99iSyLR88zZVdhpt86BiEnAlV4rdXgRniCHqnE8bmX1JlL53RA49i1QhOqiXe5TmxU%2BBShNzGfdhoAlJnDVjBOnI5f09FmadUq50YYzn%2Buuo5kFV1t0Ea1FMHgpDK%2BRsj%2Fv1UXXR8CejTMGrsXF2ab%2B11lOjoL5fLwjuK9AQo0VKPwbhW0LJNpN3sub27hWsAGS7RYuCDTg7c%2FxY3rURGEZFOceitJWROH4LHLRFGnMc%2BzSGtY%2F9Ka6iRCC%2FNYmV4pr2qOGIGpal31XpcFVkrqty0OaCCdvUhwhD1XePGAAhzq0s2H1zT3h%2Fgp%2BXCEg4iaYWhuqi%2B%2FUz7I8o%2BM3Qr4Mk4o0XWcTwyYveIiHctRAV0L9CN2y1Ok0xVD4DHj3WSz52KF0Lj4ouj%2BI9vYnS8FLEFBZcxD22%2FYxv4c1wZIrm8j51HB%2FuYvPov8HlGqd5n8KTNXLpBdihWGmD2XrIwz%2Fy2Reu3ifBgR2WTcjicB4yYSluP3gvkzWJY%2Bd03liqqgIDdBeJEv2vcqCPwdzkAIWDOwXwcph2vWuoMRNSOYnMJvIz8cGOqUBar6grWsH8Dl3deEIV7qIkSmlznWLkbk7EZ9m1m65l9QsQ7a5N2HKrMaFzj6jDf1OpNdCA69yvpV0fmrmHqBctKG1JHrf7F5w1YmyyKjxPYxXXyqmZXN6dsoFeFS%2FZ83nFL3HigNomEeNjJHRv2TqKt42md4B9%2BXAaZkzvZX4fbMSreL7tQP2TiN0s4Uw8jFYZiUNTdR%2BaD1BDX%2FU4YwUcupL6wTi&X-Amz-Signature=321c8ecc2c19b78fa40562b8abb91f1c11d3382de64368be97b3ab530f1ba93e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

