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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665QR3XN6A%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T110049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIQDnVRpxS7FM2jSITo2nJ02JIfF6zwEL%2FLuXuz%2F%2FU5cnGwIgDPNgpYmG0Ne9%2FauAvV3vr6jjFsCMiTCASuEtBvq1zh4q%2FwMIPhAAGgw2Mzc0MjMxODM4MDUiDAvTgw6lbyk7NDGJZircA4hwhd%2FLoC8bNR%2BVRDecskI2pCAIjpUHT03rIgkM6Nk3lb66g1NWYfX0pU2TV%2Bqi5VmOiP5hXkKE968GpKGDQ5l2ZSJmct798%2BsaJgCYGYCXBZh0NzkQqDMGB3wMRPpqL9kOlHkD0lPXXLYYFKc%2Bwa8Cl0QPF9fz5nuOf%2FzIiCPnUlj9FftueaZVCqWn04QuV%2B01s%2BB1xqTLuVKOpF2xq32Bg6970SpzU0qU3rbiy8nF7rHcudThq%2FEAh2%2FfoYTLxbvmtfqDou2c8RJhRP3dCWR5nwMxWUMYLVte3UTBRxC2njWMwGblhqmH9U5eixF%2BT0POM8pubfnU9uFDeDjcNuVAYYTcx67ToA3xijAn9nVnYLFe25vra8V3wzu9maezcCWsam8tLHm%2B6%2FeMUC%2FBahi%2FIDywwqQrMMeHm8GPaywxzrY5IbmtlH2e0S9bQPvn5CglMzNeNBDheS3od5FsvwGrwak18gc9pMcH%2BuYTS2jmxHLsIcHh68MMKQAxdHmd68aA%2FgdmPzwj8Zsz0jo%2B3%2BN91YDu%2BAOSKqpRDW2DGYfXU0kZylsnil%2BpM1sy1UD5Pjo%2BrS6UY8RyqPbdxqI%2BKf7RsO%2BhLKoWSsA4n52CbFgiB%2Bou7B%2BFmliomk%2FDML3Um8gGOqUBeTOfM6xCGfY5DhW%2BEQgLlgvFyX82e1qVT9hCZ5x3oMYbr8k3R2N8Y%2BOW9xnBfkFcnA4nbrb7fwKV0g7cGGaYugGNlFQiJjPAvzJNAxCPi1%2BZgfUsR%2BE%2Fvnac4TLRP0RLb6VQa8WDu9WsFWKigScvwHutY7pQHfpp9XIxM1vC2Tp758xLTr2DScs9RwD1qe%2B%2B2UeP0FgLZG1INs06XqGlMeUVqnMS&X-Amz-Signature=a0d14b302ad9d2c370ebe9c92b589575e24e46b2f2956ced6c19facdd1ab58d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

