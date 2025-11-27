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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QVDEQ3NP%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD5CDOGyI4Xuo4mNFhgGn7KDls8miihkH1aXYLGYF62LQIgB52qVgHmdD%2BN2z5U83ZCCTmLoaTSfjp%2FvOCh49P8Y6MqiAQIkP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNg4XObdxUkc1uqj9CrcA7aU28Nww0vLtq%2B7JsvYj%2BC1RTl8A4U8JqpP5j8%2FQUtrg8mOj5m2ZNH2TG86Rl9xtr%2BbDmxP2qGwQ%2Bm4CFR0LT%2B%2BhhUzdS5DVFuTj8peMR%2BuBqj9K65Km%2F1xCKLJf%2FO1o2fxmHqLv0F9ZMBwWehTKtsCErOeJojEUYNyxYraV3EmhFl7UUYVep6pzXBvGOUGyv4eom67J%2FcXYFbY4NuPqT2JBB5gMjDB0bHh6Axw4QleB2%2Bd24w4vuxowpISmgsBjvnylluD%2BIFBOqSA15S4SWcm2DauQy%2BKfcUXoyIGSoi0LzxWYqS2qYxJJeBy76YOTyr%2Fn%2F7rWDeP4C8WIDXqESU5utGGDSNFQ1RkYbckabjwFnDADYwfQJK7YXkAV5%2Fb%2BjWLaC5F1xUR1Zl4jnDwllOzw2oZ6Y09MJyUnHiscNtuAvGta%2FTFaBNZWA1OFLow%2F51JM9e0wtcH%2F5iSARiMZftvw9PZkPf1sz6ecfbKB88iTUHvBQ9fMyF1Ojt25qd2rQOO7UvB23AC%2BlHOCS4af2mHCx5BbwnP%2B%2FcDKIRsD8cBsHz1IqKxqBffYlmGxB5L4wsEyuIglaS%2FafnIUyaD0lQvx3MT6gX1JB08vVRK60mM78BDrJO6Y1vD7gxuMOuVnskGOqUBqs9emzfPbYbNGYFwXg8cNl4rUd4plzmfeQBD9HidGTVmuV18TZb3e2VHtOCHs0Cm%2FBbJXH3YPRGy0%2ByN5CIoBsVW5KlHWqpmLFo9JASdOhrRV7wtl6RZTLHFPv%2FjrjpoTdNhcjrDvv3mv47WEnjmoWq7ZNM5elV6QlTefFnKFeg7kUbn0nFPPSvY3ktC3dZsnoXE5C0uBiXMCCQ84P1w7HMkiGAZ&X-Amz-Signature=1cd3bc072198baffb29503e58b6d9c8f2585c44bcfd07cf9011e1215f23d1312&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

