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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667RNTAT7C%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T090104Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCf547U3zbnlXvF1GZhA69wdhzU7qG%2FEtirVcKRPVOINgIhAMrk9yomXXyiblDQUiNfTcVVWtG147vIEfe4nxfHrdS3Kv8DCFkQABoMNjM3NDIzMTgzODA1Igzh6Mq8PYA9UBAXVRMq3APfq6TMkNXR%2BxJDGnBPlrrrW0DijYNf79me7EJZhpt15r5V3OePo4dhlp7KJUMr0Tuzovjia2vRT9P4RGMfjV8czLAXrsCWoLUMqT6xiPMsvj3%2BGSLblqUaAiRO0oIb5lOpW0UO1v8IyGgzou73rZ2HOAFOjuS9SmACCV1xpwiTk212zWRLtxL2cwEsr7eulp6qMwFuFF0GdphFkBjzp9y%2FMvgNL%2Fj8UlptJ5wSMJW1D7z0nSh3WnAbP3VU4Kayj%2FyaVUv6ak1seFpBko050jD8gjmRx4wssLbx89UQvqbF6fvr52Xk9KWCHlidVuN%2BpTsJNGG8%2FiY9CCZ6PHf1Dsd%2Bb7ZxjBnp9MNQCW7Nst%2BBiR0QKDHkvAyZBmoE28epBgRcvUI7EmymcTPLsf01rUcN2NPyZstbFscIVtWZ9h46ZtKn5VdmGWebk59h2XQJzl8LCW6mXvq94guH9jgTAUf22rADgU9bPNC8QEfuAzR%2BbRcyzQVHAa8lJghCt9mPBrsxnh76AGWIiHMM62UDfrlYtvI6nFHoS%2F3vhTiuBZ4jyOkc89WdaBLiS2k41Zwnvv5JItgtUBpJ58R%2BZsa%2FqjZ66C3nqLNcxAZ%2BBV1eYlL6wxueZlS2s09f7wdz8zDfgLjHBjqkAVV6Hzn3wqtuEHQiWABOoRNG9mAYUObOZc%2BVp6QGVcAcpLIJ%2FWZVw0LqXvOG3zV3bPZWRQOLfNH%2FZbsbeYakfE%2B%2BlsFZ0I5SrQ7FNRJwuNn40t%2BhIr1fWc%2FzP0PrXSXwzVKclI%2FwWZ%2FHVba2xKh2oEKuQqJd9Gw54mf0pzTtbn5fQiEw4WH4OWMoCP0SdnrDNdXjCwoG8MW9RilvW8knj1RgvLkc&X-Amz-Signature=78b431b7e87cab82d4f930cc434f59eeac0722ec7a2a586606a0967267d44ece&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

