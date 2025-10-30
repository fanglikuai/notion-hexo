---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664UYQRJLH%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T190041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIQD3QIhCuuXPidDDXI6OWPA%2BfaoQ9D18PruPJRukvESVoQIgOoXrxEZ8S6vQvRqWP1Jn1N6boGKP0m4SvbTYObadmbUqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIJpRo0L%2B5SCzESGqyrcAxDXSOdjvSc1s7yruYKS5Q11d0E6Ubi7Aj0qGabJ9D2EpvM0pWzwcVlTx18O1mD4Q1PE66imADVG52tsB%2B4JqLOl5xt1vihTYPll2CfZQIKY%2BQcz6L4ScNrCLfL2F%2Fbq0TdKRBdZQPoeJ0GCoztyGoK07oXo5e4B9OBfaBwPM93EaJEATeSKBMrd29Q7mY2UxDLr8Br%2FLq5nS1e%2FARCasdJ1J7tNPpJhgFkmYS6TeKAlsFj%2BeXI9XcjSDlEzRAjBua1kTaKmS%2BvzZjNgR0Y6%2B4zHjcidHVxfwDv81clkx5Wzzlk%2F2D7X%2FLP7kzVaXo%2B1pLxRE6tKvkL1WX30x%2B9Rl0HSzGWndiAYT%2F7%2BfKmpbrd4xUzyIIbx2Lnyi1znInZZEyIcQ6x8L7fsqLIu3q4jpsaRsiODnb63pFOmtF3Lzcs9wuECvIQ5095sY1UxpcOaZBx%2Fj1GD4s68NUaRkoyBxLsRHf5IrPSUv%2B4Q9ph%2F467ykNwE9gEUbZD7GCA9l3sTJufWvK34HKFstmEK0BYx4c5vYbGxf%2FcEjwFCdifY9GYeWgsukxbCPY7%2BZsAGqJs%2BvoLVlvUGYUW0xsrjZWkF3lWd%2FVhrDOJ%2FTPY8J7YQaqBO6R5pq1FJ3MdmtMAhMN%2BcjsgGOqUBld0%2BwNLw17Tc1BYFrnOV9BRrUkxuxK%2FQQDAVY2MAjqRRhOyRehO7rsGuuAPCMGdiwn8kxc46ZAqOt3z1w97mWRFX4P%2BmgSslrz6y0%2F97U%2BxOvrXIzyeEiZMuEYNvys760mFhJoiU6rL7RgFLDAYUlOWyuTE6bOxJs8WEE6w6cEzVtVlZ1AshBgNvobHjnvw%2FnYY7gQy1w793Fo4jzZWMN%2BJMLyP7&X-Amz-Signature=071e342424704655cc937037fccb8823e7e5369795c15c1c7ada5af65ce75fe5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

