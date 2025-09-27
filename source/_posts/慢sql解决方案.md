---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666YKS7H2J%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T010040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBAaCXVzLXdlc3QtMiJHMEUCIQD8%2B5DaO6qs8gRrPhu9607vYfbOT3TeaLKQRbsqFER5FAIgTvTb2eOnoPCs4ZBdowZ0uI5aXYZRGqG4%2ByG40m4iSTIqiAQImf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBs9RdtWB%2BDIyICc2CrcA49H3pFeLCGJrDeNC0cJgXetAm4GklFGK8chYu09Q%2FmFh1nHHWAkbHF4H1ezMQjPtdU8obMpWlEzfIcikhdMv8bQ3rGukBYqs%2Ffs74rznv%2FQvarBjTuGoUq%2FiBfLl55w1YlrcHrHMwA8zq%2BzNmkoqyKdTKdM%2BGGSzsBXWmo85zCzH2fD0Tqv%2FIUHIdoaqvnCu497LHeNKVZ1c%2BGNrgF%2BM2XXNDORXWyDTVdLPkH0Mxa2aK9RppZ5Zq%2F6PIcLmbORJ8PyHpif5Lj9%2B1Qw7%2FbEwNSaes5E72znE883nXW%2BiUGEoPiSR%2BXzsgSHKAO6y2yddJYyGWS5fs5vmB8INTBNgWmktFHt1DyUWGHRLayGoYkZ92X4h2l6%2BRFWHKqMgU2DhdV4evs6UK9zfX%2Bky06kIoXuqe%2BcpRESb5M8AU9avVw%2FEvhR4KCoKkoJQv9GF8tkccJ2ElK9aY5xs6UacL8Yh%2FeJjjgrQ9Z2P1Br04WdgcRtsjwV%2B5X6rSrFXy7fbYdSiZ4K%2BiUBuDLp11uZxZnwr0jjSvPQ%2F3HvCxdJslPKlsAB0kAR2uTcpamkfckjcDU2HVUTLvbkYbsKgiYwzTULSsynJTM%2FqqC1gihWOwx%2Fyj8CVvTmMOLzTG2nvH6GMLfR3MYGOqUB7gcPu3GF%2BmwYSSezVlQ0KxEBBjcbrFfDBId0%2By%2FVvd7csM6feE5FLcbIchdbzIArl5M4idlpJSK8Qg19Ah0vjZpJyi0UZRFE0RAWGUR8VvYvBCb3TqRfXQe7wEzWfQ3pJWxp5SqLj6VRHQCL9TuS41gd86DmT6oi%2F%2FOV4RZoCfZdBT77SsRLbkuAi9jxrSp%2BKxX%2BtVSMV5g8Wa5s%2BlTCU0d3bViH&X-Amz-Signature=94f6d961d56c6c3df6a76c5d56cf0fe8ab01901d45ce0fbbeda5b165d03284f4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

