---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EBLQOYT%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T140044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCJ6tL1QI8jf0rAFOCVahXeLpJbLIfEyzDLgUf4e8JmlgIgLfkadmb1f0ONqwakfwZyVR0nyf%2F617kIzPdOddOFXK4q%2FwMIRhAAGgw2Mzc0MjMxODM4MDUiDGN%2FWRzb0ssqUwdJmircA7%2FpFtman7k2kTSPW7r2SMfn98Ol5yVp64WEtH3TY8zXnau%2FNxUTJcMsf5LfWdyNj%2FpooNq5NSK9Q7tgIWUifWkMNXynMP2g3r%2Ftb40yECBsCzpao4CXGxhJKZ6wer6gOMEgaaR0VnGcO5n8E%2BffRq9CNZPLH9jBwQLPLy0NXN1pYyMn4aPXRKQ6b%2FGXHr%2FyXbSPcHWpVMBsc4wcqhISOX%2BcdAfbPySVjb7eUYijnPXK23RHurzwzC%2B4d6Cm4dKmBfHhrZxGCRjMEFks7wLEHez%2B3zCYsCkccGOOTuNeUoiDiPzJ0Zyt9jSpxWovaRVEQ6h%2BiE%2BvRGei05IDjq%2F6aUdoCq6zBdTeLxbPn2FFZWoRSY3r%2Fsz5X%2Bw4xg32hCZa9fK8y8r3zV%2FF3zRTFSqBcZtwQ9jnJlDgsRE%2BTqLU%2BW45PUF%2B%2BqJsjNEQNeTMwtKXoiQv%2FEnWDZ7rjdo8e2Sff8v3H%2F%2BmbIiHa3panec9yawc9r%2BlOk3xb7U7eBIXI19RvhPQZtlug6R7dMSMD%2B2UdJ8jUrKSl66rMTTDsaEWVXFl1NAV1hLr9x3gmxOkt52Khx3AP3%2B2Wjiu2cQNUY1ks5b8S40djKUFHPWUd%2ByWb9tIcSCENRRq0%2BklNrqCMJeyysYGOqUBcKJ5ERHWlV1kcFkn28qJZ0nPOhj5dalJVxa5%2BNyG6X6vcQrf1N114E%2F9L0VF4hVLN2qluPPfVdHY7nFnGl3bY8OKkk8B8bBfG2FvgucWVQs4bYujFnz43C6msLScbFIdLM1aUPm2BSJOhI5sJnC9Q7hEr2%2F8gQNQ8wl1Yz2BJqs%2FvrWAwmh5OQN6VRe1r%2BSkrS65OlCKvfyRWK9nYVMRBupvXMRn&X-Amz-Signature=cdd23c4da69f459be9f43d4a5a720c2b8aa6c6aa2155e5446fb4d1b9b1c7585e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

