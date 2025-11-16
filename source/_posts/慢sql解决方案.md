---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S3ZFIV26%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T070055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAGknMPK78XRXByOrVcp6QFUo9L99jzOqKual0QNMELJAiEA7Vu8Qh%2FKHrBfBxS%2FErn1yKsatVVf43vQg7ErDJxbOLMqiAQIkP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGwUa9nkTQNtcEGe4ircA3iOJ5tTqZDKp832EnDF8ohM%2Bv6a02B3O%2F6Ki5dzmPfGcA6Sq3T8cSsq4VrzTEuYrnmxb2dZZHFdbnVedVbd4aPk8t6dD%2Fy67%2FHRTep6T54nLZsIF0qNSVhCSAUuiQ8tB31RH4ZVNifTTAb4IntvMVpMpnTbtGQK3w4bk1ZFa68ATgMC03sJSLX%2FMrj0f7ta1HGVTZ2aXqq37oEG8JYMoUJ2BpJp4rr0ImV4dFNkgjMUeY4XwvsMIhGu%2FpfXDsRaBsvwKpMJgxaGpjtOr7u2ZQpPS3YjQ69OiiT0oqnmp1RdiD46vrZD1eAk7Ej4Ylz%2Bw%2BY2TTr5EST9CbAdxSxKwMjdytx%2BIZYZ2ksF8QUXGPNgZiR2xGdffbew6YbzYEOvcWA3%2FeLyW6Y2hqrHa2Yx3YKcXSrKUyC95NAd4Rx%2FPjmaM%2FUvMHYPImHGiu7AlxyDmqmROM9wQFd3aCndOpcsJ1TtSguTyqu1Muyk%2Bxj%2BdINeBRZF56XTtAJHHCyBzxiR9eNkL8DUeOfFRx8wJBhbFumbNqWCfyO2jLyPjEM2WXfk9yB9zLRH4M5mNgSnqsKE14AGiZVC1lvdCrJiFElyFrEi9EEagWivYdOkCIgD%2F0j%2FuN542qiJncBVuNyvMPbg5cgGOqUBH2gHBCNwHlJ5SxrYo%2FE0vBv7jBxAlRTYONC23wr7AflpW8HwjxyCKbb5MrlQLJGoscA5l2xJ7Srgkf8YJgr0AdO7gP03QxBZgcvsrpE3adHH3KzP6XPbdDqao6KK8BYf5jwtjYWCMKeWwg1ZS1Sgi8XXrNHMMCY9%2F4G4xNCfvbmgdUROtsu46xHXzHNDd88OcX3VO5kP4j7afbMCtyFSJiRDHte7&X-Amz-Signature=864ee1fd3c88079754cefa291f26febcff3a97af3fd8c1845118a87db0312aea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

