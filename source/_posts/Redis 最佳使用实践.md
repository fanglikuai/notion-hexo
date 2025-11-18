---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHNLD5MG%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T120046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCDf9DXVXm0Nis2IM%2FU%2BPwanCOJ8b1iOPoMWXv%2Fbpz5xAIga2QizYqBuf5XyZE%2FeVLsPIOPTorr%2FfZS36Ql%2BIS68ukqiAQIxP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCQYouPbjm21SP0wAircA388%2FSSLxTyhyv4aLrnwnCW9QL1BaTBY1W%2F%2F4ZVZ8OBEb17yO%2FJzCk7zrmNka5XRacht9YlHNdm2RrfPbYw7YwjlsSFZ0%2FzfUv1agQK0rUmf42mseyhoygS4gdgsnOGLn56mqYTfRSV3zrPLg%2BGlyCclKgjP2RSGRfrz5AYIkvti0qvfmGAo5oMyhuhTmnXfZDO7Cu4eefE4ftg1olWhOmXF0V%2FMuKgtjdp0O9WqYRk%2Bdfx1hqDzXZfnTN9Jw9Ra4KvAZmGBl9re%2FZT2ZhgBC2hwE8ZgvSGu5kD8QxWlKLxKOb1SDCIPMF%2BM5%2Br20Rl2efaab1c18ufuf3HvI1bI7%2Bi3tSIT%2FCWnk68AGZHkHRUS7S2JSeH7SQpM9JLUDFv64Ir3OczU%2BcKB1FbCLPy4hiCiF0ty5rPBCmLF%2FwyHxdK1g5RXjh2hpSRkMHpLua2TQlJqbhM11IrIfhANvqW4U7ZOalSLQZLBEgWqt28bvzZRGypY7TAPOS49ofVuHm0pEAb8z%2FvwlzMx1RL4AVk7X8fW6kPGdY8x9eEzbw2jfpspMUr4ImbhL0YpRsFuiP5ynXxpmvGZ%2BJJ5xFxRvS2%2BSwnIoWpKavLvwpNdOwGUQaZ1UDjFeMwiNEA830hmMP6k8cgGOqUBnm0Ku87FWNDhK3CGZ3DpJxVxLqtk4zRueKziQOJWy%2FbeN265P9IOES8iTbCEkovH0426EUwFiAImfnI0UNDpiScBormu4ZnLzB6CFrotSx2upCN0LV%2FXdrqPHhHaBn%2BJY9DtvQl1Wyj3hoSDaNf%2FM2kS79LTdULd5rnU0J7sGQRH%2FPjQ6c3zq5%2BgQ%2FCXXqWYvhMuq4Ibva6IcytzuEebnHLC8oyy&X-Amz-Signature=241f8010f73d46b5e7ce7f35e25f887b263e5db6b3bc29d237199d3ef2e9662f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

