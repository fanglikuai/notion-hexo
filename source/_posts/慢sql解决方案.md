---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q2EGBUXN%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T160046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIBEfHgWMuS1mR2hS9k03D%2Bep5CD4dsDaqbcK67I4%2BeSSAiEAqOQAgnOk38%2BKY8rqmKrdmOcWbkjeH7T56LpRWqpYdHAq%2FwMISBAAGgw2Mzc0MjMxODM4MDUiDE974NuP1mUwqslmFircAxOaXEcPqctS4HESQ8FFNFXYEiecn4QuMHHaBih9ires1Lra7ZK6CyXrWy4t%2FW7BFJ%2BRBzt50Q4RKAzk%2FRl4WfqBLB%2BKnOHRRU9f3wvvDxK%2BDR4m4ol1L0h5MZDaTG32lwhJVDkrQUojPx%2B9Dqb0cZM39DqL7lFQ%2Breg0ownLJ13%2B37WDaVJI0%2BQ5d2XQ56Sd%2B%2FbAbR43%2F4i6knb01SSylwFOPHqxL6VPbMMmk9s%2Ft5IGMnhl6LbQ4p1GGmTyeOnSjjcEKHo2%2BNhW5nyPppC0VqGHFvKlCvZJza18ICVzWlISI4tV1yc1mYbt1YHjsGPxhWkxk3H4RpgPDZoxnusum8jUvHy11Qrq8W8GtJ9T0XAwTsQ0bng1p%2Be2%2B%2BiYUAEKSN%2B%2FEnl%2BIUcR38aZP%2FMJZBEP4f3xrA0iGjXQZQ8AeOwrFjMcqZjO%2BTcy8jyAN0xsU%2BYoDloUnBiayOj9Vk3Ge3i%2BJXHE0MyboG5rfCeVcHMOQTmyQVXU0jYYUdLEzFphqnh7Sq0f3OTvpHSsyFNkgtqm52IarpzRvIwOgtQdS0mL0ykSrEAufEBrsxOlfGZoH%2FT9YmZ%2FWz7eQDbhUV8mb%2Frro1IKR3W8%2BHSksUDQJNZ%2FNTAtIP27igIJ4GyMJjhncgGOqUBbvepcVcJmHlC6Z0IzL70tyZdJiMNNSpSirlf6fXKagxcZM%2BG5jXYzOeakVphlGjdNlFs8x8VULDC24YyLSeqzScL%2FCF%2B90oDASh6ka%2Fe%2FXUHt2tQQOlPkzDyLMExo1YvT%2B%2FY6AgW9M5mfP9VFvqFwHOzUDMvFtpOF2IJTSwoqufdT8KNnp3jWRqTGLeNXKNS2mdI7ZD5indUQLV1yNoSkWqnEKI4&X-Amz-Signature=2f67a21aabe39834622ada105fdee9a0a2152c3eeeb5d97e1429c09eeb507e21&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

