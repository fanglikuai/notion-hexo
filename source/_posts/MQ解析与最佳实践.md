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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46676VUDVUX%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T120100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDwaCXVzLXdlc3QtMiJGMEQCIGXU10SPV1F4q9eunTe1cqyRohWLF2TpJ9SYEwngnP58AiB83C2Q4FUOe%2Fs6EIWMoc7VyYIBV7fxneONbkND5G8LniqIBAjV%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMsfVSbNR8T6kaykOuKtwD6PQ%2BhX37pNHfjRYp2vX43kRliy7ToPLjTgqe5wWzxpngt5ILm7ONIOBcJxJ63yWa8W84UC8LezJTvYhkBi3UntM7zVeLje4F9XjKTUz8tBV7yNDSGf%2FJooiP53wA2lDmfpikOH8X4j4Po1HOyljWcEyauZXroc4RwD%2BFX1q5NBhAWOQ936SkjZ%2BpzQSYNj3K0iZWHxj8sqkS%2Bbv4gdHM1OymMGPM8cpItgzVOBa7G265cJQYC2JSobfO7N4%2B4CKlmNpfNf3r120GlxstIvhXBZOpLWkCTFoX%2BKZcZ%2B8Ji8h87RzPdlY0683Y1UFeRGd5g%2FWbQcJR9i44td6wV9c4aKdA7Bi9W7o%2FBXyzaTZaU27T3ctSHfil2HmFhPBonQUu1qKbF7Qv%2FUrMM1yH8kGdqAcKwtCm%2B6JVA%2FZmKgtMfCulnUVpC0G%2FrwWjqT%2BOdvDCjV%2F4%2B9mfKhQnMWaYD%2BGauuQiD%2FuvD6ejNkyA%2FQFHR2Dp1TeZ8zHGna2fZQq%2BgxoaFcWV8LA1AosMtMIYfeFp7%2B2uV7dYIdJzAY9cj%2FsIuUIjk5ArcIPg9ww2Jxo6sIGyzHF8flKD1uUiJEm4%2BMZD0L0Z0o%2Fs8HzaLyJu7uT445yAWDm2csHHfjGaVqgw6sGexwY6pgHymwf%2FT5Ijd4w%2F7F1sTHikaxCaU0uYC4Q4lubANe625ZvTqf0WTdQ5gTMepMq3FiV2wHaprp4KwssOXQyza%2FcmLRQcRtsrAP%2Fb6iKYfJfdk9uY1Y%2Bf%2FGNlV%2FAeC40i6X222KxmnQIk6Z4NYVvdQf1F0Zh5YojdIs4OXuY87b5Iw75HblX69c0wwveLMngPQcov0T%2FCxLNAZDYkEZ2cqEEyi7pDdng0&X-Amz-Signature=64393f54e9c4d5f4a2a5dd5c0caf2785c4ac86fb17c108190bf8b46b6db73777&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

