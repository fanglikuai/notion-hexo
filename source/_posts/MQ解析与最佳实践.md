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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WYSBUIHK%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T040041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJHMEUCIQCmqfs30L%2B1OzyNXN7a5UfSvBQlh81u8%2FrPoOe3CSx2swIgDWaUcsH8cQUCA5wntR%2BNaa6aWY4W75DdOOFfNqppyWYqiAQI1f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIeN2naGipNFPWfw1SrcA4jH3r3GEdxGm7z7wi5QeoyKVGVfWjIZv0KpEJCfJw7IfDvuUoyah5dwrS%2BjU1gfCWXjJd89Nk30vSsNcl%2FdRKIGajDRU8GFnB09A2mK4xRK9dEzGq8Skgt42d3nP7pkHE%2Fedx0tC6AcGnjiu2NpoQMKGnulyTp5BzAAkE%2Fj84HYUg9WapY0KVrSeJCVUYQAECzSomAGOP%2BXpAJPxLmPMBCNF8jDTZLdeKiDIwihfNkT35iMbm%2FGRieeSZBJa3S7HENAkXSQyisEcbHPTxZyvaO8v3FrvGiAaYsdPe2picOmrGEjV72rjlC2hndLN09WnyU8gXYiulA1Pyden7KjzYW1dAc9VzsrsY3M9vDk8%2B3v%2BFZARQ4cEKF2rFx3AieDIKHb6ejO2RgwhW4Rv3Eb4IzhzQp1aDxp%2B4b5mrw%2BlO2NxVsGbIJE5HGtWsMhuHvobibKW9%2BNjFG9op2qC%2BOOmVkRI96q7R%2Fpn82sFH%2BS3IaMU%2F5V%2Fugs4%2F0tb8uIQZmCpWTBArg4w8j6GgT2%2FjHddVYMpOADwdRiUg59R42Qc51yw8F5gNA5SGGwlfUmzdAuKeWlbGOudOqbRqsIpP4vlo4rxakMyEd0IA3un4BIVuw6J1kxavCBzCGkBpj9MM%2F29MgGOqUB0EAzaeSomyZWq1USelx494I22PxCsuXr5U%2BwAScahfuakWRp7ilncyt1GfYOPmC7kY9jhKUDUgrqugU3%2BNeIwPx1av5sfPz%2BsBGZIblp4k9Y9zzpaaMaldsDpCinIwgreNU6w8M%2BRMEyrovgj6cs5xmTSwSAi%2FPlPEtU6IsHyUC0qgueWtp8mEWUxkuhSmFIod1BftG1UsoHn9gh0X7Btzhh5noY&X-Amz-Signature=d105ef83e1664a649e1faf452597a449fe2e04de6b24f66d6ab1d30a7a881239&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

