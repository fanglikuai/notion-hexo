---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WNYGCL55%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T100038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD7GErKPnw8nvs3zWVAxZ%2B6KsEYjtRgv66GJRDSOBA%2FAwIgIkZQebgmteZkWBDK0jHRsEES1mFbpkemiz7ZM1av8EAqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKOV1TZxBbPsv%2FDhQCrcA5aOMZ%2BwTT9qxKRjsGcpASIbadr08n3LxwxXsf%2BW%2FoeRHAK4WEE4vZj6Ij5WbWW2OeJxJCBIaIeMbtTfTKo9nvikmwl6dkT74FCWnJ5yXCQ%2BJ%2Fnf2DPUypjfSvwYIbzn7xD9tWNEU5fOZHhgi4lcbPHwAhjs%2BzwYXJAhd%2FfcmdsPCCHzbpwigRuJM%2FHG7jOPhMxcBWfrn1WL3yHGWp4R1gKOZIZkzeZZTH3MY5Czpw7XTGxBKkLPGaluttrrCyU7oxtdR8Z9Dr69cR4dR0Ja2aWUz5%2FyYb5DnhZO5yOh%2FyIZ%2F7HBh3G5ItJf%2BBQQI6Ck0Gp1LNEWcEMUB%2FS3iB1rDo7i9NwmKqI%2FmjSxNn9EMCnK7xtn955S5WmPaMCAXdvCTWo8N3871hcnlEVovcKtfRDMiHucrxxvMzWPToIZyeVHDzFUyIyxrVmt5fe44mnN4SgGggYXLF7leT4PLWUAqjg8BEGvzS7Wvre8IOi90gEViaFIQRdO8gdu9hcxDG0p897om5BhG3qN5tImWUlGEOiX9O43m%2BZMin%2FOL%2FHCc5uwbMfWLYZmK7RrR8ococOzSw3%2F%2Bn5m9mjtZbJPgWw98pdH%2FrKOR20EJjWbcXLNiGYcy0KrLdil2plsDw1iMMb%2FvcYGOqUBZmQgQXAhvBeYhsJ0Bk%2FR2AzURv836R7rH7ieLzF7GMP4ESMP%2BAz9cTUdbxX8rHpJxVsmznOqrpDHrBHHqp%2BXJBsIinBoQiJCbyqvyRO2Ni6iejhwYBgCruhCcjiUv4h2JLxeouP70S%2FYZl9MxU6arFqorkDNy0bM6kCJRJpQJv4U%2FobAXCfqev%2BHzbmxD17qWo8LvvDsuSQAjZ4tLSTpsMGP87iW&X-Amz-Signature=7ed3c1fd9faa63b6df6f31dd765d9c94a3274088eebf5ab9056b7bd4f745d4e2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

