---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q4RBBRUZ%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T190046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEIaCXVzLXdlc3QtMiJHMEUCIQD6hOwCggeHD5w7NuNthLpNoxs%2BfS0sKhVG3051etKdmAIgDwsc%2BDVUUWU6WkvUdfy9k5%2B8%2FAJheag0FSnWGOtP7s8q%2FwMICxAAGgw2Mzc0MjMxODM4MDUiDGzHC7JZW%2BPpF2GVwyrcA609%2FIHP2h%2B3heGhDlyQVIEh4lCCGn3AmVMmWDDmZfoB%2B8VEpEiyJay8Oa0eM8i13gI4YmthVPmJEf0lOeb19uGqimFO%2BVtMtR4XeHWi8qcDhV%2FpJbYOYYLfE5F4rYuGlw6scevCmD27QqYuHRIpXBiQu%2BxrpCphTrfR4KGACa6aajp9ezOCwpksVaiIqOvpKkBDWcEKf%2B51pVdwOy5EF1kfZ%2Fa6pC3nclqRJ73iXLcCH9fLTo%2Broq5UTTB6anxvMgxDX4lY9wB4ZdkX1HQPep92ydnXJU7xOqo5VWDw5goIpBusbiZutMrVGndAIkhvrKr616bjxWIERQ8JS0hsVsHvmVb1iMt8%2FYYZqfEdJM3BQ4U3FJMZ7d7%2FLn5opSfz7n0iaqi570EY2GCnkYR1xsCF25OHM9vYWN9juftxILqCbORTEN%2FKccxrmHBi0aMnHyLhDR8yq%2FHxg7eImDueBFAIdxVvzB%2FhORQeiavhg8iA25TTM7VuZ5azR0Vdn4MqQiG6fDRGzhZsP5a0PhB%2FN0RyAt9r2%2B56%2F1HiTZ2HJJeMteU97BS3QPwZd50sE6hA4xD5oM1tEFeRUtKCcYccF66UVEZwnzfBLNv9pVAoihDEpQV7ckrps0XOrNxWMObLyMgGOqUBS9vv9B1ua6penmmEidaw3BqBVwc4VrhqyldZtRkwsURCuWYuOG0RZfB%2FBKXl4lsuIB%2Few4cJuQURkPL67qYSVQW6g47%2B5giawNEUjkhwJWM%2FVVpX6g5SrvBvUs69P%2FY1%2Ftv2259huyzKIZrB4h3XyUaNbE7pYHXmHse6tCqX3ZTSTnd5vWWOJkZo3LxpHIimQdokLOTOYAAI6ArazbQTmsHaptoY&X-Amz-Signature=6796ad61532fd958fb528c47b575433405a81422548da065c0d0f1d818f0a3ff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

