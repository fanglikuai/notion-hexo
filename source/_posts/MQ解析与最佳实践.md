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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHTRAEBY%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJIMEYCIQCc5cXmVBhUrU7gNl2YmZU5AoXOqvAdyCYXnQ%2FLegTKZgIhAN434K7g%2FekByjzCjVdy6jPyKHq9UzgZ%2FLPf01y7I%2FwqKogECOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxrSFpbEWTyTOMRpeMq3ANY8yUQFC00EfvdEnFKOlwl%2BJrhGnrMT2jVQenF5h%2BbOqY3t5PJta%2BvZjcjJ0quHCcbXewWvshh06005KC%2BTwdb18kRDgVHdssdfzBJqkS%2BZ6UhVIbyCy7x2Xttm5NcfqGuqRgk5teMpSJ%2B2GOiSt2KBf4ZdzsILQHcu3tj1Vl24bOYZzuZ86l9N0TraadWP9vrDwqsC7AdQ8yT%2BVlo7lwxqEmIacHSTsrfJMU0v%2BBRZkzGdE5CvkTpnLHz3ZbzmV8AjsqkT1Qn9BukiPwFoefeaRPKX6UeNee43cHGg%2BBkQE%2FRWkoeRC%2BaKoz4RipvRvK8Zz48X63zop%2BUH%2BziODl1YHJhztV6OztTN7H2kl9Tr0N9swZzBy4%2FHc%2FWnCF4OBJhD9Yd6clMtUDl9k%2BGeQ1%2BjlsP1xJLCr%2BuSypQa%2FLe8ZAE9ojAxrNRyODpNm2NHrsx7VYsx2PcMHiUPorXtKc9cMOZuT9sgWgsdLLnasez%2BjvsKMjSskCdQDKR4T5BNzFYvFEQLWE9Gz61sCxcbgN9EvqY94QJy64d7Ev4Y13eQLhEAM6C6r%2BDZwW2z01IMIxoDJhjVF2%2BUTyIIO8QLaeXG%2BHF1DljkdyWb6JrhI7T3bZZS8lUbxweTurS5TCk9%2FnIBjqkAbO0kHbullT0GxOJ6HNkl9owldm8XPHnGG4IGqQWAQSLpx8zm%2FXXKus9h3CLCJjLXLVOG51DcxMeLejzbaAUs7GJm5tky97tMn5aKR8V7ulz6E%2FDOhGtjqkB7QwkkVz58IGGSzlrZKyXJB4VJoARz05jznDE3zcAP1OLDDH9m8RiTOYR6oeTTGPc%2F%2FP4NW3%2F8Pe95%2BUyTw5cS1vOgkbocS8k%2FPgK&X-Amz-Signature=7ffed44de54c3ba9ebcc011fe0f519396cde617cb9aa76f6dbb1e88eed34908a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

