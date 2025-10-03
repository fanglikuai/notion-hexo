---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663XRQNOFR%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T140050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICj3tszmmkkQtgyJb1puE76S0DMPZlH6WFX78Qq7ZIG8AiASbIlLdERVZChYvPCcbMryajCGPJEUJwR9e3U9GTvX%2Fir%2FAwhGEAAaDDYzNzQyMzE4MzgwNSIMYlYvuD3C7mVtP%2BrhKtwDdJV%2BqC%2FD9hTxVoiXuckj5e7lyUf4d2oEoFq2%2FbnKUkwUtZsbfvd7Y8T76c4ddzQXALYQ18nMBzaZ5wDUVnczBHBLaNIecu47YjcM3K7yGRSr%2FzLj6MSxL6fvQdifdWDwKSUReEqR%2FIzqdXtFII69mRoWj4goAZ7RutndoUa1oESpLqqSDcEyI%2BPsh8H97pA9AeXwtX3Nb89OoLlecPA%2F%2FOknDRHMHZsjdoJL1%2Ft4JtP7S1yjVWpPb1jKAHZgbY3XIAs3fxcjUTKdBz8GJz6e9Dj7mIifXwmg%2BkcZGSk7mzZWoP22wrpHuSyXcdV4Zlnz0PwNtO8NjGGU20fAMVdFLycb4LJaAN1tmhpX7znv7BXJ0V9CqvC8%2FsN9n%2F7EJHHLfHMNMqYsuIzOn%2FBlwpuyhmkbAdG0j%2FmwrnHLTrzbd8webqEsjnaKRc%2FbIK3nBx9oUqcLpKnTR17%2BliZhYGJk3YIzwCG4p8zo3q8XtB8VywiRoji84%2FxNpy5rKXNAwB8B2ggnIcqJdioY24lf05x4JaATIZRYWLQVE85e07JcyMSxFOQduzb7Q14Ai5bv72e902zE0UdtyqxeXxzB3GSNdqnLPJOcxLMprcpc9Nc9zX3LLy%2Fz%2BaslZNcKKAQwmZL%2FxgY6pgHO2P%2ByrZeBhqfK6vYH1MaVNhK5Eeo0PTDUsuBdHGGeYdPqQrIA42QMk5KqY9SLw%2F6Ki5xQYXCb64r9LUyhzFCIoXOrC8UmwEU2M5UeJHAy%2BnKNSqCEYXuYpfMiyLE3QIZ6zB5DahKSkylML8JIuQKEIeoTRabifI8%2FrZSWgs3FjpcxgCFeVjkmr%2F8P4xuqATDMtwcftOBTzGErsEpisU5ZoY7SU8eT&X-Amz-Signature=8016428f8bfb09173ab45aed7131839930b0e706f3d62f98be5e22f8899a2765&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

