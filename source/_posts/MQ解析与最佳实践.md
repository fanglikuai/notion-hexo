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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YKFY4Y4N%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T140049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJGMEQCIAQeQ3j27OTiwC0nldSCMpjdju56M%2Bvz%2FhDmBdOmowVTAiBJhEXE55QKLozFzPWWso7ADfKE8qMuwbs44uRjYE7NlyqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMyTs4Rq1fhk6DpEVqKtwDSr34XYZrmTzLDcra7VDEkizkixxPKJBIzzDQOw%2BR5hfgpDbZKW1m3jXXUmDjsw7IllOiWe3DnXP1zOKywEmiN11HFYv5Fc1NdXqPjOZ6zSmNIDmlPDeFp%2FKx1pS3VpvTj96u4qXxeMpTBzNF0W62IHYq9iCmD8%2BsiQZZ13D24mQNusPobxiiacWdA6piOvNn8owAEtRvLw3HvAXd7%2FtrTyUBwszYZFjmgcQQPT8pwYUHSjtcMLOtbPjMq9mMim6Ja1oxQdVuMK0abrYH9ruTkDpwtNq%2BA5Mu5%2BgHPrMBPQaIus8a5s4%2BKm7bEeEbdTQ1jACkgYtXzlLFIwWCxgTMZgMEMTWKKq9TezthUBgrh54YMe%2Fkjxmu4J%2BxdrH2iZv0u0kDsQ3D31l6ntyPtwICFdR6MAWf058%2FChKCcUieH6%2B2cZyZsOz9GtKFMd74zF6z3kWXwEu1jQPDlW30TX%2FqOB5SSUditTyDAxHcYtkdc9vhizqzHjWN5WO3LXPywqTLHvOaycUi52if2eWRJUHVDyfXIi8TVFFN8haoCe2awhpQnsGmF%2FeW1jUNq1QpL6ip%2FvHgX9Ip2N2xnSK1eeAdjv9uk7futjkr1Fdx%2BDZ3QBos1wE%2BiemuKNDjFZswmYmkxwY6pgHQv7G8zDYZrNJkuIkuf3oL1lkzSGlLUtlPXu0MOtsLh64TOI8svLjNHD8zUoAyIoRsEaWg1o3YyZzjijRQEfhQkZ0LePhiyB35eMFakmzY3yo6kENs9uA9Ppz2%2Bsfrh%2F81dBcuK%2FeMuhjgaStjBTwcWfAW1FMhDSEFPxnbgQExyY%2BwUluryKPxAv0wek%2F0Rr2D9zn4he3%2FmJoFgbjORAiTmikUvEv%2B&X-Amz-Signature=c83a43dd56f7ec8b8d8f67cc19dd663305d4f2d8bd9e97b9c8dbbbca6d10fc44&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

