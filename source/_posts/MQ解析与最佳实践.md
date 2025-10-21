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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PVMD6FV%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T080115Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJIMEYCIQCQY6imCtuQioLiAM8S275qQMs%2FFI4Fc4F6D3YHiQQnbAIhAI8nlO9JdtneCnI7Pr4VIJDdP6KS8sKmYpl%2FMYDL6M8BKogECP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxivMYd7uc3uRwAYw8q3APIaAhaEbui3Loi3goUfJVprd%2FHfiIqfNzDeAlSD2Lvby7e5JOmynZGiTh97CNlB8RrCb4vE1KVSK%2BcNpE00aLbgKXhBw1TVn0i9YXo8fUtD7y6V0O2W2y%2B1icgsu%2FmAzpik6RgaJNwHd8UADVfOoJqIcQgHRMdToUP2JJOTasqesnkAdfG5r8hL1jrISkHC11agafrnLaTX5DnQR0%2FZBzPLprluQLkJAnR4oUAsjpOxXnRZH4waOQBJkdFfCFJB2JPwXDnzj4CPOaijsLkm8Cj0YQFpNHYSBmnQ4nasdPTfaJMEM7ilXeMI9C3LCDMgWEltw2RNh7EKjDuWBS%2BV78gti82lsl8aIK663ZLNbHuEOIZGyXf0bOxlt6fGn9Xw3ptiGmQFX4ULAzjpOXq8BzalqiEibih6M6vI3xMSh2jSgInIrdS8g1eCrAR7wH4b%2FtDNpcnVKbW6tQS6B9K8YHSMmXfNHhTuwgy6Cr%2BFuWvABts%2BQ0bn1D8WYH38pFkYj0pAHhPPwW07rEJrfVuX4pCxm7gQXxej7NisVrg9dZBmDKVAN5AsHL65DDEoNBofAqnyjdGMTBHz8OFa2HHg3VhAAH1N7S58Tin%2FUpeNqg49paOlW%2FQgphQgRvzwjCzq9zHBjqkAbgjKDsNkqV5JTkcH11aQXJ05QMUOx6jmbI2sFkQ5%2BLwq%2BWGW2BmzKFZlBlNpSiKYnMrtzxLiWVNJEkQwXbDjX97wEUcN6EmYX5kH1WP8gpCRk%2FZI76ZyvBmgPWSiC2JYhzqfrPTO6HhZFiFs0BNa%2B1VIcGZTh6dI%2BZ2jG7Z%2FEIGPl0xXgA5HIHjkVbiV9MCuaYCvxYePQX5qEOnGyR12dErGZfA&X-Amz-Signature=3ecd6794556b89ef07b5702ec0b90da58bddab9a018602d2538bfc7de6dc1d43&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

