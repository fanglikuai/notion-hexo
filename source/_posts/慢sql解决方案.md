---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662BNLV7OE%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDUFQY%2BIn0k%2Fmjxuk2SKMjQFDYLM%2Bb9KRJ%2Bu56gxOwdcAIgHwC24J6g4VOp8aZIuIdDbfZmZmaopDumRoWv8TSOBj8qiAQIpP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLUtvWVOX1xCZjOrXyrcA%2FxqJRec5ih%2Fmxv5Fu5VmocCfTmmevh1WTjHywNm2ipClw%2BP3TSinTQCWIfJ3ZuB4FQ74koihpBFJDQDJ0EiCBOxjbhiSoOhTwNgVSLXMLbw4Hgk7GbFzHrpP1NR%2Bi3pz%2FllBcujA8X59zS1L5FOOW%2FOfAyA%2FWxrcPn%2FrRHxVpmeHPq4H5yTt4kd46WsmEQ5zyoWd9m2bpXAq%2B1OrS4IZCBEAyedoNyJnjhHbfKIJ7E2ipp7YispXd32DWY0%2FbZvhHUSNcXjN8PcObrvP6StSJkwNwkQOfTe8palnLRyA3Rjn5TjaCf3pYBhVVF9i%2Br1I%2FSLazMIwMP8rPmZnILhUozrBqdKc0zuh8JBNjDJ4Yy7N2CLNvbIp2vdbHAGXrOv%2Fj9xODpGLKAfzzmWDaA0K9VicBw1kQzKMxQtE2PkV4ipZE60aHeAbfq5r6sEAOSbaEd%2Fpki6ZMWfIIithX6Nix5MHuZ4rl%2FUtegYRZw7v7u2iELAOf7dwa6EvHrI1UeI2t2pTfmaUsXqI%2BTfoZey2jDRfLuZCfTLcTiUOQ0VOwzaj067Tvzm%2F%2FrIRaorKe6xmCTP7GySHpS0QqY9UHtUDORg5R2w0qaHNjOQ%2Fn0MY3xoIZSrJeeQ7b3soSrLMNmQ6sgGOqUBW5ZHiQ8a6MsLy3XLj0Ulvvk2psdKnG0g8MQG0v5L6y8gXY67lIc%2FkUP3h9HexusSnJp0VpjoLK%2BMeD8JSAjkl%2BOGc7%2FOH9pts2KegFQDwvq26ETvpupwFGOMwuVhSeczeIbQ0VhFHbfFTnIEl%2F3ZG%2BXckLczaiCh2ibF7m70B4F14HJIK841ZtKK577d0C1guoOTapU8800N8MfaC6wyscx5duxW&X-Amz-Signature=67702bef0e1dd74d36033cc42f54365a27a2f2236d264ba2e583cc3c36c7ea00&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

