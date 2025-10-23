---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664STOKOJ3%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T110044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCIA9%2FJ3kybBg5c9BaHCggq1BoZRPtegiOyS6PTxNq6jgIhAOzUSVsOAmDTzVT84rE5NMQEa2cNS6XQFUEe%2BgwdpVbgKv8DCEMQABoMNjM3NDIzMTgzODA1IgxMYypFhylZ5waU%2Fb4q3AMHQ%2B4%2F%2FKRJqUrqWrySy8wPugg9kyyx5C0L%2BCFM9UNgj3RPM2TrqqN29Ge8Qe9lCGvM9caMzoJC4jKiyEFKPr2Hpq2f7ENCilh1pzu%2FrpgznJ7YRz3P0ZfWuf2ho5zu74oD1YYEnfDZGFut2NtiEjRn5iZvdc0sVd2qMP8jjr37ODTqJq4TyY2Jt6mcEnk4gcJAl%2B9norKwOBiFRXv4dKzrkZvx9aMD4E3%2BZDwnKvm1LtxZCF%2Foy3HuU%2FstIjCblFVfkBT7xw6DCTjmKAsUJpzHr94MqqwPQqiOJqeJ8cX0fdJyj7OfqRRKGRlrPdiaQvwDg36tuVMrZ4sXSDSwbXUBWyxa%2FUFQLSbvYGhQVs3voIo6ei1wqwuWl9BNwgTA%2F6HXRTTgqeXlnvQO7yvnSudfYZeFsq5s6iFI5YKo5yjiYy%2Bmbpx4KNDgLxrTO4IYnLzS6ZO6%2FZl8gIP5eNlZkmPR5%2BmBLXPDq3afb2SxqBSVF2nc8PdW9PyCL%2Bd5yKP0txXtA6C%2BVVnTE5yEPOi1FtttbH0tKv1GmIT7gJSqv08w9pjvgDvSJdyR%2BJ8T7t0X%2F0Vk7eZD%2FhDTdNaKUyHOkM8Efd0MvFSjqnSR1KYPRjUatJPQsjYAWHj7BQkUkzCX9ufHBjqkAVBOi0mnG9TsMcxjKfdqELUfU6OGLq%2BZmdD1L3%2B9SwPgZw01BHWlm4M9Go7c9h3iZN%2FvJ1fpK794DPnofiIukxx%2FKlUhVzMddqhCRAr%2F%2FMF7dshaupL9X6zGi%2BOvaK8Hv9ma5%2FI5Gf6%2Bo%2FTXuQ8Baj5TI8Lq7t2k6y%2FyqoHlsh8T0cpkGIQOGO9LyZdtzIL02DkbEnWTV3yWqKzgriRh%2BaUYAIns&X-Amz-Signature=8caec082cd384ea5717a9402e486cc4c674c663711d55d5226f03a9140940762&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

