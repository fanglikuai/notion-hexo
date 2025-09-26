---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663FKH57VL%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T130101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJHMEUCIAMjC4bpjl%2ByrgR0fsVmWgy%2BxBvM4QrUUqdthRsHjR9VAiEA0ZPSdZZ%2BeZPRwHYPLtSOatebEhv8et%2BeHZx4CTB%2FWnoqiAQIjf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNYCFVWxbx%2Fkwa9RUCrcA%2B%2ByzQUsWOld67fkhxRt5H4s8Vy%2F0b8FrNz%2FmGa2qEfeLJ9t2yzUMBhssbDbvXqkyFlKciuwTBPt8u4QSh%2FHnvZjqbfCsrUZ3vk5QCHF6dl1Vu6k%2B9xJW%2B%2BA9GGTumXO15DLC3Rgu0Va3kp4OacJVAp558vR9MNrE2q0pcTHoNmUwajfJkDYm04CNB2rz%2FZCNZH%2BW2lnP1HrP09tisKczyu1ewhEutGA7W0eeLW1dBwDrnOP4mKFvdpRMyooY%2FZCSBRjO7YDEb3ht8XQHgPuvcfXlKVFGitHJAoQymOhWkw%2B%2BNgdxy1AKsLIUeNELEHN8ee8KZp8GrfHoFtVhCM2hlcNfjNMpn6GjaPgeeA7WY6sTVaqOOxRscTq6RVnav6V%2BbubSCIL0bp7%2FR42EUBCnwNgqzwL0XdV0WfZivT0AciHHOPRCVD0IlmsolHv14ZFDAHB%2BL1VZPvloIsSzE%2BUtpGOfFpg%2Fa0HMxrN11xMlevefGdiNrKRbm3f2sblPzMbO%2B0TqDU83wm4VYQK%2BYEmGemTZPSFT21%2BWXVrbR%2BPHTj5J2nwK78qB3up%2BVE0dL8QJgoZigxAxHzCyMHqmea9%2FcJc82ncSUEbnaq3WCFpBfvP7N8%2BJb9hLjpYR%2ByXMIr52cYGOqUBPPClQy0Bti7iyMqVmvNRXqlSph%2BSMxXynZy1HjOqCVb1RDtvwYF1NruPB4josjpv5zfJFZ3TrPwlgMTEulFz%2F8MaA2e%2Feu8de%2B98%2FHNoiegNg9t8NH1LX%2B26pWUkH8Jnx%2FleUBHuAJn87lgqdC5ScefOgwMAFaLdMteNODqMIB%2Bq8wdmNwsJf1lWGm1lVObb9LKx7w7KZy64IKv0q4cpOx1EDWrW&X-Amz-Signature=617fdc131a203efb1b44d859ae4b9c6f675f99b03f459970a974a8341eac2525&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

