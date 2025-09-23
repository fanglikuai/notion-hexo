---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QTYF7MVM%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T160052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFw7EK52jmctIC0DXq3VOigSMcce3Qd3XlLguRZ51e3NAiBku8ziz7rBFDpwSH7Sr%2B8QChwswKpszbUqlN793sVKvCr%2FAwhJEAAaDDYzNzQyMzE4MzgwNSIMK8fN4%2BAozVzCMNvtKtwDFVg0fFMV%2FMNYRdnvdU5M6A6SBRQZ9cPoJIDaF%2BxUdiQtyOMqf70Z2WA1p77CLUhCau5vnO9PLdUsK5%2BIfYbsS2YTUTFV%2FqzBqN8SQeRax1uYr9AsF5AgD%2BIf9RcN%2F6saN7vfZPPtoTayY5nUzbaF7Qn8mb2skzGAs8QEJh0%2FNtLXaY4teSiRY3qPWHujrIZCPFvnmdlD2aVO3sJnD6GGCnsPSrqp0GZXJ2k%2FtIePC%2F1mnj01zUJPNVRINelrUq3UikiPMS2IY7Lh3AjM3lo3e5pDxozgwmVcQJ43wsPkesDU6pQ7fOx2XP32L0QJpwWFPT%2FPsblvOG6%2FWVRPG7QuQYEfOoxkjtg46lpG0ehBqjRQswbMRmD8e5YudCOHhFI2Syp%2FeVYTYE%2FpDEg8G9g2lZPnYbKR3b62Y%2BoHPjEk7HRZPyH%2F4GOd0NCVpOv7vJmqibdAZ9h8ENitfs85yVtrGMqwR4lMnpZqYLXlZ5Mvp20nQw49KB8Z2vDl%2B4oOFvRaADZ51XZ3orGMnSM7WwpVsTmurhaMPhWHc1uE702cgxlsUi9hwivMVb39NmgrLne86aznlz%2F238EUNjkVQH8NcxHxBfXK4xJysc8aoDB%2Ff7x5ynYO77gphnnqKxkw2vzKxgY6pgGtQcbhCXmATf2Ccm1kBn3ufq4WxeaQNmLX71Z11hk1hLE1VjB67U6GgI68WKzQ1UrJRFJI%2BIpeVxxwcrFSAGRHra7JU8yEEE%2BlfkiQ3GrRNVWRmXKwfKYLnE%2FTVsvWrjsEELVVrySKZ04QbD6VLpZC6uWgnoQk35CnXPaTbxItwsWsmlnt42oRrPnI6wa1uyJu3tD6DzPsH%2Bhzp0bt%2BSHS33AQ7DX6&X-Amz-Signature=eb1eddec2f65b6e28fae9374b0b4fe15dd1208c847eb4b99121fcc2ea1b891d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

