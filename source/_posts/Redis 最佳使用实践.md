---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WNZWFQTG%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T070039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCmEaGOq2Adg0ZUjovQopSGlcUNmxQuM2nwP5MdYXoIfwIgA5xR6SbzO40bLRvqmsa0XQ8PaZmWrSeXJMtJzU3tKv8q%2FwMIWBAAGgw2Mzc0MjMxODM4MDUiDDTGVzAOL%2B913B9bOyrcAzvXMC9EF%2BwxaqCbJiWANyQ6CPqJ9LMc0x5n4zb9uEMSnC3ip21gnwkkRjG96SEY%2BfLTqu3cWk1Kp4080Vpxnwr2Wn9Nc%2FcIbwTe2e37s6lLTRvg7IjsSXTTzrTVyZOGwmgUDU8NDvzPqvYZ7mcAB996AStqDknkp11q0UE%2FG%2FukJxXEsCaDtqdyxI16GkSPW9TsAMWWH4bcqAEbBf0s%2FmfGueBOkQKiKYAJ6rQrsld5ey8baKhx3x9Zzi%2BivX%2FE8nebLhYgk7yYiCnJXGdO6zu%2BEMf%2FnJaJ%2FQREXPOYCoLq70UO2eeZT5J0LF%2BEuXZpu64zOo36oWUZZmbBULNi1hNmPZSTOVLlSXHUfDxfIOKe7NIVknzdn574fJOj4CGWewvrBrIdE%2FgEKZbwgXVFEbyx3KDT2HwdL5S0pLLNAsWUIbapKmUS1IHtnkvn6jybFZwHOYgRhEkbYaATIcC0zEYUDEq99uf1et1JsZzaY7%2FVaZLUx5nqdNrStfcywrsG2%2FKh%2F3Fu8xYUKt81DWQn2auBsAE%2FZWHuE5pRlVYmaLX3bLkPJSzjnc2DVLUU8QSe%2B55sMc5y%2F8TW4f5Za3K8HG%2BBZQ8CxvxcR0s13IFjILok78OT0%2F6SiSSdNZLYMIf8gscGOqUBoTHSZkyPfvVAVWTJbzWnQcf%2BgzF9EZFuJS0UH%2FnnDVtw3czTx%2Bu%2B%2B%2FF22wuMWAc99DQIld68pUi46TeRrt8MURUlP8q4JnfCd1fSRtKzcjnaojM4e5mPZ9PBvVebL2ifKL8CzkOy3CtyQpDxaDKySjLcn6yZATAI98FoEhDCWcApgf%2FHGQaM9QAozjaxrn0sWkeWm4LfinM5NSgIecsDxAzRuKh%2F&X-Amz-Signature=3a1f64f830901732f571a0f7b204b04feb42680ef9e6e88ee6fe5ec6b8179a20&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

