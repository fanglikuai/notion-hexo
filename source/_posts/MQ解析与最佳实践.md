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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UFYQKDKQ%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T120055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJGMEQCIAR%2Fw0RBLXrwIcH%2F6X%2FAjCDesPa2A6IS5xjCA%2BhalLZfAiBIUlTe91bzwoJuX0TvuGfHi%2FAFMkmTYMLyWf6d9yh97CqIBAjU%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMjc6E9I04sIXG4MdTKtwDeqaUOQz6na%2BVK%2FjvNH8DgNEt%2BTiwK9nlGwYqcA5usZh%2BWiuN2CwwBJJP31ba06LvAxEowdooaHxPX8gTo%2F99V63FneNsxQiinaRoopI650kV%2FGL2gaEEWvJfU7DOTNZsdYvCLYbi8muKazsjiHjROHQ6IM2Liw%2BzrYkJ%2BvxKMf3Rn9VZX0TgwXXzhGjLfH19zOVLTUqSjjRdQEFBmP1WMe9IgIlVlnaU3tsUckc3gBFfm%2BkvyYdotrJ%2FcQAWiy7F8l4Bxgp9mHaCraodC6Td%2Fzkx8M6IWuZbSHuC8ndygIq7fykr7n2wKwxX5PfwEtAjR3KQA3KRz2N0EGd3Fqqhkz%2BMVIAEKr9kEbJw16j8w%2FQDd7I6IRPRuxNi4A01njDltQ9CmCTR4qRk%2FmOlGHdvphIprzsTw%2BSjrr4Hn3n9wCJqQa6KMI2%2Bvk9hs5ImwsdHmqkU1Dqe5uKZ9M%2FbR3DhlrAzbdvqpRcOZFbrqvpF4xw3gMDZWNv8XCD2rEqbh1axOwAvDPwTnIQAfZtTlYhuK8srM9Mph56m%2FhEu%2BglMkoIgaAbmWvQl5tpxYPuc3iCW3iKRzqmFGgiWsUrW6K1%2F%2BRGYTwam%2BychKxHBsYIzaaoYUzo6EuIMvQ2uJf4w4em0xgY6pgFuDKMQmYrx22buqaULjdYFSl3spUoHaX73JnM8PLLM92oqurmaZf2HApYs55yKt6MCOBmKWYB%2FhghC38d384SPUZAdGhQVy1OeMtv2TtNONxF3qG%2BDxSjxSbCPRZgRk1htfMOSivWNUgJdjmR7ZIBqhpHp28IBblqR6O%2F9WZk94J8%2FsAClgDxw8TSLnWUHYprdfmPF1EX3aspRcqhOUNOKlP3faSW5&X-Amz-Signature=1f326258df0aa5ebd3cebe8f4adc9ed563d3981bd7f43029f067bbd745c734b4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

