---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMSQWIDF%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T150058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIQDtftWTgUGJA8PY57CVOscx%2FTPeMv7u2smX74hlywN%2BrQIgPmVjyIGtTI4n%2BCP%2FQ%2BZI200zOHp7lmmLkWZl6986sksq%2FwMIFhAAGgw2Mzc0MjMxODM4MDUiDE8U%2FTX8zJFZTNjHeSrcA1SmpUzzAAz76hUTQ1wLw4YLWus1pe%2FLDHuJ70U%2BhCnFswzRsmi4lD2sj9X8PY1uob0Cp289yYY2gIp1C2PV67G%2FACQza%2FlPYY1zn9pLMcp%2FU5sy6hyMZ6F5UU4rateD%2FXKsM8VEXmfL%2B8wk4zobioSkOo6wJ%2F18TEUImvLRs5fjIv3N43iT4aOJ2jREnEVuk9qbZ7z2CWJZye37xJ64wY%2FfEZ6eu5tCv7SmiwPxdGzRPyWrcrtRzWwC7XEHNnZym4JxLQTzBB9ua4MAIJomiWv9XBWzuHwkOr0l8ECEEEjzFGH9dRAf5BcIIkC7RmUGdcaa%2FCbt91jGNrRFIngMJ5AWeFd5cobqWsA2cbxxN3Z3CLePcVRj%2BrKYiKA7veJERnbDLgLViVXDIp6xK5yG%2B2AxnUef9j4VsIdNUCFAHO%2BXldlinvVZnkzO1uXIhgXgSdB13L4l3BojHm2Lr7XzgcFy4RfAqnFrki7%2FmVXnNHnDBsaKWxQJZUCxm8TkOsuPaKSlTTZ3qQaw16YPhv1ZicdQyViRiFsRuc1xIx1gHQ2G3tgRR5TGkR0sfo1ge%2BrfQ8Cf4mduGMKy9%2BimmpzD%2BdkGWuOkqx68Z0Yp0aMMJni2PJHzr0AhwCyyQ%2F8XMNGkqccGOqUBtprjTd5zks%2FRsQhxDduAUWKSHIDg%2F%2FUm0SZH5VqcdAhABY9rGgg06H5bgx0dyHWBJbCYv4sFml5fpqmJWEUmcsCDPlTfMmyRLk%2FkoHRSl612e%2BMi2u6YEgdOkGrFhoZh5F2QGvJouUntsyzXpoqm%2FBq6%2FHjLoTLl0Ph02IjPRteprR3vonyJuZ94WebYbJwJ%2BTmdep1orQgju%2BHsmECMATjyELeF&X-Amz-Signature=076ed7e2464d654fdbd86a1ba836e3772a4cc9ffef92f8ad339f33a919e0760c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

