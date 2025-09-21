---
categories: 整理输出
tags:
  - 分布式
  - mysql
sticky: ''
description: ''
permalink: ''
title: MQ解析与最佳实践
date: '2025-09-04 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WJ46L3K3%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T040048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIF9WpGMsrtNZUlJ%2FCZM0NUZwhVUcp3QYGMm3S%2By58bQtAiBFCXMP7ZSc5omBzYMh5PMG5xDTFmWy0djoH4epzK2TzyqIBAj8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMxIrAbK38sbJAJl1kKtwD%2BQXfcre4uts4UMhZW6icDp9WimO%2B6ju9xcSpdO%2Bin1Luflmx81omyWy8GYBf%2Brb4mMGakFQULpDQVgFEIYvDdQze1rK2Crs6T3pNudp5rKo72IFBuWR6aer0dghSqtzS5AtX9SQjVyjv922xeRtwVw4B7edMjm6lGgTV86blJHPQuVg9TyEAF7cZL69SvvT7kmteDk9L5k3oDS3mRPmP%2Bw4vWZjFm1ZJyZ7BE5n4HAXJ7dH%2BSFNPNn3yHwhuH%2BVLb1%2FwRgdzkPprv2jl%2B5TLqsT9T2FKFWdN%2FYMiZNuUudV8ohi3npRHDVI7ypu2rC7GYW9IbBnCanCLbdRAMYijlDm%2F8JerK446VNnytp9HsB2bdTsguswWBr%2BIH0Uagj7R3nujS74doDEKKd2Yz0WG4GKiLEP%2FAalBC1MuprH4Fv%2BmRiZABi%2BE9N2YXyQZE825EvnFsK2WqysuOVAVlUbzMtOiqDf%2BHXBa5FJ9SBsUNn9AegbNfEHwhgqm5NsVwIU13O9cLTMmgIDHt14Zl3VN8x9dFKKGp5s0W6ghj07H5lDud3mX4fJbxQlxca4jYvpCv9%2FniNF1qs2Mml5OEsw2rnflxyqn5Z4XcFMdu1lWRcF9De1yf4b87GRlgg0w%2Fdu9xgY6pgEcO50AYN6Xq8XCMPAUZ1ZM%2B3f8DVBrhfyEJzZ8v7KuQTXt2WTH5jYawbEigJDoLe6Os58KSkTmgK6gybKcABVKa%2B8wraRJDEFqDxqpgKWVMgJRMm%2Bg9fsoHwIzjCRo5sp9m6E4rpmuLk33m8tzCtg8EyLc1gpZhoKAPL2zGVdcty1C8B284OMDmsLaA%2B27FuWcKXspXcllQhbzwLxoJwFbxQAdbzbL&X-Amz-Signature=caaec726a1d3c31da85e5ad015da5d8ab1713d8b30e593d58fdd0a8d6116f1fd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:56:00'
index_img: /images/c8962001455e38177108499d1e1e605b.jpg
banner_img: /images/c8962001455e38177108499d1e1e605b.jpg
---

# 丢失消息


## 生产者丢失消息


使用ack机制


异步监听：

1. 成功/未成功发送到交换机可以触发一个confirm-type监听
2. 交换机发送到队列会有一个publish-returns监听

**但是一般不这么勇，成本太高，丢失概率低。一般都是采用日志/邮件记录，手动维护。**


如果使用定时任务那些，成本太高


## 消费者


手动ack


应对：

1. 消费者失败后，将这个消息存到redis，记录消费次数，失败三次就直接丢弃消息，记录到日志数据库中
2. 直接false，记录日志，发送邮件等待开发手动处理
3. 不启动ack，使用springboot自带的消息重试机制

# 幂等性问题


原因：生产者没有收到mq发来的确认，后面本地定时任务把错误日志中的消息又重新投递了一遍


解决：


redis中增加唯一id


# 顺序性问题


使用一定的策略，如取hash值等

