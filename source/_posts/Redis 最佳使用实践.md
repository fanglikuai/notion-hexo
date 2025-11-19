---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QEJGWMH6%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T110049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJHMEUCIQC4u7gnK%2FF3%2FjS6pqFV2SL%2F76GlNo7F6oexMwI3FEjTVgIgNebFh4COiSFZhorftJ0PYS1sbYhzUuh8HFBrV9pC8NAqiAQI2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBsnE4FMjWnQKC3%2BOyrcAx1sNWwbKg2m%2BqwT8gVI8euQWP2o4fsQ5hhDJH6XdZ4P9BHmQOmcHnwm0lAobg6qcMz0wk8Vws%2BcdlQ9Lbxk5ODY6dl16wyZSCyasl28wMwDyMXpKrdmxIz3XQdPdRde%2FmuWLo4iCIVpk3xZVa6yssHm%2BpXzraUyPwj1tDUl2lLC1S1sPwHM7NfaFSHOUBP61pcM6BxVQqn27vELdSkHlTnJckI54ig9tva1Z1KMvqiRuUrBakkZXSvTC6Qa0JTBI4AlMQ022Z35mHj44cDXLYETMBENRK3uz2TNRhTu1MluJggK1I9WakMupdP3QypP%2BZiKC13gO1GzmLewTwW%2F%2Be2kgibPrAiFWqIxAN263EjVVgaOXhh4Nruy9HcTSDafxxyS309jR7QtuJdz%2BmiGPRrq6eE7UEDPYrZgoLx%2FPUBAWc3TW3eUrIv39R4OKsoT316u9kwn6MrKkN9Lqer%2Bz5tK1niqxFR1Rbbj2jLqVtuMT0W%2FchuGo1w8s9mE7Xr0Z4fPN96BSxZMIAvfMRBDqWF%2B5KA8tdtu%2BUWSeV7s6sNo6CrziRHtEP9WUoDDo0qn5H1m5VbZzM2gmPZkpFTJAis%2Fon9IBsL5MwfDcgp2219RHcYVhcq43WdnMqDGMOK09sgGOqUBhXjC7W5wMoF43CG72buiMob7Vbbs3bDAkx%2F52wSWOBOB5HTnJIOTR2WUJHmq4cGXFyrMhmpkyPa0tKioQ05H2mA7XS1e%2BFS89Wa1n%2FksmbrOzr3ih%2FGbxW8XekJgvka1nLardoSKGnlBymsBdyzQeHTOSmRi7Ar9SNjHVPYb7eHmTSTOdhffboEJBSrBiGG9HEK5LfhPLYgp0JM0ShBbY%2FRqEAJV&X-Amz-Signature=2fb9dd66910324a7aade5dd1ee3b50622e76d5ee85819db7fe9667b16c0adb70&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

