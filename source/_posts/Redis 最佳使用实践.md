---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667D7TVIRN%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T000052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDzohNPtDtlEfEdPdDl6E%2BVFwJzj9RLSzn0XlYymvRDggIhAMYtBT8TuIpNw2lW%2BJBcuAQXQyGnnlljVy5PbZclw4NKKv8DCFAQABoMNjM3NDIzMTgzODA1IgzYgxBtpHPOLltm8d0q3AN5OAc%2FhmUSjwxJypT7mWyIl%2FxAZcr%2FmGiadVPEq0TfzZwm%2B%2Fb1Dmx5KkaLnOIWs5CbywsdKr5R1pQpQN1L9dvIZLeeW5FMYmxcqsiIEMKyeZ49Z3Q%2Fgk%2Fv3e4e934tRVjBx%2FcSCt%2FomKpO4351n8WrvRc%2BPo28SK3ulHao4Ke4Cvo4KO7WDn41jfqtNGcVLKk9crk0DSnlhEp5Cyd29yRIpUzTG25%2BxtOJrU7hV2%2FHluqEHt9MBzimiZ6f%2FKwdER%2FSGO1QDqIp6iCZ%2FymMeBgyHeuY%2FIfbAHHVkrgZCzYG3CZd97dzVve7103e4SE%2FcS5cJY7EB693FdiB8mImicOuBtkZMXhoYpdauwmrOPe9V%2FR8dZ%2Fx0%2FruZSRB3YKqWzhmemFNUJk38LNCnT5uRjKzpRp6TSzS1f7%2BNr3Ts0Hs4Bb0rOP3lnhs21TE3qeQhNArgNX2h4FUvvfWdY2PlVdvwsuoWoDJ1E4lMp6ZB5qVYv3TPz03VXb9588%2Bkn4Th4eW2%2F9GDgiXLbosU7vceKOPwnDbRy%2FtIBCQTtdJUO3v7Y6ikcOU43eOk8dnfifdoK0NnktzckDpt0GQ6Ztqz3nZoZh8v85yPxy1ZZ%2BDd3tKmVwrf57I0UUI07zZxTCN6erHBjqkAW8cOuyoSk8I8k0BqSN4GmBYXxGiP7KmSZZmNVbktNwWL4WpFm%2FTOHmnzHAVO4A4AcehAc8tz29oVmWj0QHbobRnk%2BDCfYYm%2BKpXick1EfSWkTXGOGO8%2By1nOT5%2BtU3ttwp1NYdJ%2BWDzzaLcSRugR8gK0rg3K0Su%2Fu7dPxY9x8OzoegtuYY4YVeQh%2BL%2F07T4KI1TGbFpjXOBdYA9%2FYSIcptSdNL0&X-Amz-Signature=51a7523b48a4648a08d708964bc10cddb57b21827256363b220db1631db2a820&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 23:53:00'
index_img: /images/4f27264a7e8afe769a5c2813552aa0f8.png
banner_img: /images/4f27264a7e8afe769a5c2813552aa0f8.png
---

# bigkey 问题


![1753077336565-23eda3f0-dd0d-4865-9f4e-b536a19e7c9b.png](/images/c6758344cbe13f3ebf0f8718f40ab3f3.png)

- 使用离线库：将 Redis 所有数据导入 MySQL 然后进行查询
- redis-bigkey 命令`redis-cli -h 10.66.64.84 -p 10229 -a xxxx --bigkeys`
- rdb 文件扫描
- 生成 rdb，转成 csv 进行分析

删除：


底层介绍：

1. redis4以上，默认使用unlink命令
2. redis4以下，string直接del，其他类型如hash分批删除子元素，最后删除key

# 大key进行拆分


采用经典算法“分治法”，将大而化小。针对String和集合类型的Key，可以采用如下方式：

- String类型的大Key：可以尝试将对象分拆成几个Key-Value， 使用MGET或者多个GET组成的pipeline获取值，分拆单次操作的压力，对于集群来说可以将操作压力平摊到多个分片上，降低对单个分片的影响。
- 集合类型的大Key，并且需要整存整取要在设计上严格禁止这种场景的出现，如无法拆分，有效的方法是将该大Key从JIMDB去除，单独放到其他存储介质上。
- 集合类型的大Key，每次只需操作部分元素：将集合类型中的元素分拆。以Hash类型为例，可以在客户端定义一个分拆Key的数量N，每次对HGET和HSET操作的field计算哈希值并取模N，确定该field落在哪个Key上。

### 缺点


本质就是取模，需要在客户端进行操作，限定取模的数量，不够灵活

