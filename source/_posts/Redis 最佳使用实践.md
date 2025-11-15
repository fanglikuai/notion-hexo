---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TW4LCSM5%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHnNc5Y9vLHRiHAHyrUN3Oo3owgAEuuQsWfekUzu2zxMAiEA45i9CFOGIE0E%2F7IQhvJi6e5obM4TIogZ9h2ihpwKE8Qq%2FwMIcxAAGgw2Mzc0MjMxODM4MDUiDAFY%2B5MJTJMLkam2bCrcAxDvumrow51LOU8CzQ3dVQ73iOvyXJbrAGH37fWddzYy2QGW6YADMJqppXvqtKKHHvSiMHPqngHgq8TIhSEQhM5QfgWrWoElwdFbqk84xk6nN6DbQuHRC%2F5l%2FMq0uRtRCsvtJyRv5v8hg0cZkVCvEg6TegWp%2BOfUWMwUi79BG1%2BSZHMmv4rQ9d1xGLlUCNQtktXswFBWlBD480%2Fh9QmE1cWrfyqcX6%2BBVunwaMc9nFEi7yMa4PStxEL1WUCdEXjwMzpFOU6J7Z8%2BwVYjMK2sC8EaDCHyithEXEpuGMOXSm0vV5eEPJHmfN1m%2FnjGGv2AQyYwkMvZ8EvhTBypzUKXCEnwVmOSAnDWYOBKTLfygxYA%2FRzkHa21WN9D%2BRCRno8x8tT66qESVSLvpGpy0AHZjQfl3w01HTW1Jq2VwBcmOfybgEKjg6R9dlsCHx%2BnAywi%2FhKTXUOh6CSOJQ7YQmbwbo26Kc92Z8opAYQep1sVxpxaHMtHpiqCQkXy%2BJVxZtYTT%2FAeg%2B78JFEgqo1vpyFRFFbvqi88VL6SGXJRnSSUK56zYYgpD2HldgqW9rnw6WUk2MjKECc59%2FV4%2Fo5wdJytA%2FAHo9klCYCyPnGpvbng6GQQqJmgmI4lr8nwAqiCMJGu38gGOqUBSvPr4q2mqEa6md5PrOBxngE6aK3G7Cik6%2FB%2B%2ByaEkJ8nhTv1dgEf4iKrfQYSV1tfkvCs%2F%2FnSpy86ScpS1DAgvdPRhTNVHaUfWzmMzDmgyhIi8WLeIFsr6o0xRdYLTnjIg2FAo0uGUBZIU6AmsG3UU58roPoVn9a8L20b4Exs8ev0xS%2FbCmvk2K7FqumdJD%2FL2jhxD9jxk699A9qePvvQbnZEKuHL&X-Amz-Signature=a6d41845eb2baeec4c034ab8ec2ec04c852bf363aaf3ed50006218a9b2d3d6a3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

