---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665MCAG6TD%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T170038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBQhJTuhuqDmw25kk5CmrqfrBxIqLNMxVA7a5ktIU72BAiEAhBT6UJDF0eLSLWbz1wSMgIAPoh%2BBAIkYsHDV2jfmnp8q%2FwMISRAAGgw2Mzc0MjMxODM4MDUiDAygsP4%2FkpCGvWPplircAyk3s5pKtVg3f4IgXKAJMVWEcrURv%2B%2FyT%2Bp6WZIxkycwGPRrnYUu%2Bme4L6vDcGWgOMOkbxmMbRisfeZK4g9W37ScD%2BBhdnvO8CJrHx3RdFQQ6eHXySPHtmBd5tFnkUAn0c4%2Bk%2BDJlgx6RNH9gTdypU%2FNHDFwq4mQexvaZoyCeY1d7ii0Hp2M2S5zhM1J2jIxE0wWy8vdEDBvhVoXJuKWWSjwcyXVC4xMGWRYpneKhQO1SepsOBBeVaM%2FCEEcZpvBZBQ2ChO7pHFOtpkETFUc0BGdNJTSGk7y0aJQI5bWp4LGEpkRKwL%2FSfM0ZP5XAZ0iXDucu4PnyBuw6vNkGtCw6RrA9Zh4nFJFL2fmaWwLxzd5P1AYnWodPLoNPO6BsRQ1VRqJWDiTVgh7SHaO6LsbY3AieVP228EFEdenRtgLjJpzyyI8rlK9q9l3mH43rjy1jP0AfBHSdHPHXZ1oIMKmjk3w232ZGZu1XLYPMj6d%2F90kJBvV1VrXeJ%2BsKYu01kolrwjtJ9iEiyT01Ch9XcCPeoDHG9mWUl6LKQ%2Bc0AKnClUU%2Bhtq6%2B7og2gS%2FTwgBswu92h5aNl%2B1RSDNOQhaRJ%2BnhtUKq%2F5dLtjaevzOTDvA7azsEJ047zkgnXmufC0MN2DnsgGOqUB7u%2FYkGzJJ0RBjeO%2B86PvP2808SJS5GZy23HMmL0hMY0gtVXF%2BIL%2BwKbzA0prG2ysNAda5MMiiFEKWuAXO0gZ44%2Be7v36yOgF81uTgfJ9TKtsi4om3nqJssre4zQ00O8DgQAD8gxJ%2FaEHlkgC6W3AG3E%2B9%2BE%2B71uypUlTgr2MkprMQs%2BhKIklPrNGnWDycckDhz5mdFsukAZfo5yHVBMn7bj3M9vi&X-Amz-Signature=acb86b76b82d058d5a58f8de115a05da382c40463305f608844df5e70b454c9a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

