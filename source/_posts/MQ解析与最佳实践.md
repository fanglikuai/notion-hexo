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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TH3FP6BP%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T220053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDrAnINkiKuMGMbb8iaviM8oxxFsHJ3NJN62LKd%2BgGG8QIgEGpLfIKx%2FAYPadWCVUrADYzxou7HIM57HpKQHv5P1uYq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDKxpGmwSQXfKpUDk%2FCrcA8aKCF7idn1gn4FfnRoDZ4lzwKD8x91xCq43zJbO4n1lCaFnvNdr%2B1%2BqM%2B21NPCwZAV40iT1W0J0gw7QLGczbpvy09O1vDFGRMLNFAJKoC8wEOeMlG%2BPRLfxIf9kSBG%2BGpT%2BA86ACGd23zUw25F6j4OzGfl7XPN0FtQB6VWc5p5GzMRFdQaZ%2BeEqZIJIleLWN62wefR%2FP4B5%2F1qL6WlHc9%2B90nHkl6LxW2tLyiy%2FDpmsOIGO5I2XzEA08XOZSDkQErocQnSooAZa%2Bcz4Ay%2B%2Fn95UlxxO%2F00BrobEBDAOIpD%2FItWkNFc7S4X383gygIA7cljT3c6KsVqX7zp3%2FIp5l84VMb9EpeHJ9eZzW4xX4fjNMckN5iZH2onwkAZTv%2BIWGs3qJZv9RmzX0E4Gq4fQHNIt7DSL52ZzGwJrF3jZRe8zvCpKYGOlL78eCJ3R6lB0%2FcGBfZMs2x3lJcU2IVBqcnLkf0kX7relXPfC1aDFkjnH6cccyLObbg4aPcSfA3eousP%2B1Y2LLNG94ZgYYNkXgr%2BnCBZBeqeP26xqLBTM30ZF3fvPohWBf0rCrByZ33yAVRfk2ibwvRLiESVuFHx8BgCdQ5uP4TRywPwA9PUXOx4iR9S7IOIOKB%2BMgClzMMn788cGOqUBe41qSk12UgVw4ho3MBLwkVkYfULfaPwEaIX6inWGUNt8PhIENWUC%2FBiV3so7ADfFEdicRGUFR%2FaaEEIc2SduAYCPm0eqL0PMTqml7EVlXkvoZvUU9Qofi%2FbdHzrp1lndtWA0gG7tGps%2B5H%2FJk%2B%2BYdm%2BinxWrUOVzBfCiLu8%2Fw1px9zGk%2BGbHYIhX1HCcoDUVlKKo%2FSfQOXbPFhaoEbpVuIwyPn1H&X-Amz-Signature=b0af88c4d988eeaabeb2d386ac27b720a60be4745eff8c31a4e4f7f1b2a5d0d8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

