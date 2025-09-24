---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y3LVO4WN%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T020049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCVIrEjQCVZ3eh7wMiu5sMXmxV%2FfKxOiG%2FQv81dsO15wAIgCsBGEsVoYE0lkBq0Ue3o%2BQJ0rFSqvthQobbW%2BfNbuxcq%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDJe3UrzQDreg5e4fmSrcA6yZzOkFzOegWLUtS7YeeoctyD3iV%2FQsrwZUE%2B5sHn2QsESH1%2B%2F11bqkkDxGeqI8z%2FN9cpH9pplJVmDIqnEBL4eloLhrso7y%2FQaTaF4fVui%2FqGrSo%2BwChiXvExw6TAPHYhbx4WvSty%2FGpJ8G8ZJIqh57wh9Sob%2FaLHERjG2axVeiTu1gwBpb7f8P0L9x%2BTmHdQD0d0AkGR3c82HNJrcqD%2F25OIV2XOmCPFWsbUNEknXB817a4S3rtRfG1qSwmyAbeyDXqog2BwCLUJehwbP%2FjvGUg7NQxHaqwgzCxLMrvV%2FTdV5WSNCNBXQQFJz7%2B5UtVzU0l2xwYrsW3qgFNpLvBHmHb8UWOMD%2BKxzky4D2bxvTa2%2BU4pLS5ZLlkAAK3u5c1%2Fz1kvQvC998wRU4%2BUyhIj9YstkSPiMS1kPv0ddODwiHgtUKArYGhO7p%2BsGhMy01T9KtnmWg%2FdyL%2FgTxHort1uHEpGvTi4CfllmwbTuf0LIjS0KacQGUFKqUnXf8KmLAVUpMan5Y9GueD0CeoE7dVfFsdgta6h0HhsN%2B%2FdDIy5QhFAqL206ffaqsEfBNt8uSH%2B6SPn8Q1Q%2FNU%2FmrM1O6I2e03coV7qJxhL%2FLXZ5YHZgRFfsCsZuwRXVPr0JTMPyNzcYGOqUBFlVWpnHLWUcw%2FY2N4LW4MgSyA%2BkO0q%2FxINX3M0xy%2BIcLtT7HxDP94TACREg%2FXpdoS1OgYp4cQd2TM2q26mZ0Lmf6LqfW5gYrRUylL9ckDPqej8zDHqdHfQUPaSr57supLlFMfHkzowmBwNzw3wBbDWRPaSwWSiBweLtACZuLeoiaOpwPpiOhntc4MmCHNfWYsLmIlh2RY7Qbmf%2BgN6dEzhdGWZva&X-Amz-Signature=fb4503fa65e8637aa9993d160fc39d1cd551fbd39697c81debb57937a314a26d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

