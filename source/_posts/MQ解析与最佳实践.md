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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RAO45QYD%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T040039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDQaCXVzLXdlc3QtMiJGMEQCIBfOFcxJDiZj%2FH5nKGElNa%2BI%2BajqzCrO2snqSEG6wv%2B5AiBszIe4cQI6vjKJTjhGMTeukf1hjSM2xPK1qwvL3u06IiqIBAj9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMKTjC2iu0IgYoGFL3KtwDXw4kkjozUCnKpVfmr4WgtUZwC1DfyLPv4hp%2Fo98zG3UoOAMe8RMuEayY64NVpahinLn3XWAeYRUUxrWuV02ITwC2hGx156GkGli8twH1Ze8AHLwTPm8uQYgMwT%2BcvnmiWaTJmsuFmKgZhH92bRRcWNMCW4vHEUNJf%2B%2FC6MBBS5NgpKo%2B1yngT3QzvVruaneHSDUZ9OKcU7Da1mz0CCEacCBjxg1YoloZigi50KHWE2j8PMasZ0cWWrNAffpUCR4satSJxO5vwCeuoPJcXpiSQL0zkW043%2FcNNaElX1RPDQ69nH34To%2FAaEvoiqAJx1QVvbm4XwD0jW3Y%2BPGGu6ZRoJXi9ePpJr%2F3G9KDe3jroqsfmhbnllap3C6WUhl8S92yMnqoUKa2VW87lJ%2Fj5Ih3J3O67aumqgFa%2BE2k5yAe4jZ3Qyn9PK9LZwSoN8sP9RYP%2BLFzmacGs3Yz7Rdwgn7wnzeMuJssOg3xh07gLM8g5tpY%2FvQjApquwoOFDOPe9vRpuSttFBuCj8MmtGwZ2vMqGGKP5nn5BwxFy5ulOPhTtunh3vjKwqsaHywfIUqIpVL4vFn0N56Uh6w6z6h3YtjlAD%2F7%2Fqi%2BnvWILnMJ61ow89RZ2eYyMKGtcnQDyzUwpbvFyAY6pgHSEXSdKF0W7fn62VRoT97i3NL9Y5tIUPhXuP7ybw6ZSAhoACZCLzyBbTYrJ15J5o9kR6U6b2Z04zElh2QiHGGj%2B0N0TRb%2Bov9Rrx1C4aDV8QjvEneFx1vXkH5V%2Bqsxuskzqvo07KPZK6O2aaoDghIJzhxnCV4EcTilB3ol8Oht7tqjdtP%2BLNwOq%2B9%2FmxUlT8vACtdXxvTNLmHPaLdGi%2FsRsf%2FJl20i&X-Amz-Signature=61e4b1fbf304eb2197945266b9d81d8d7c1d753cc8fa3bca41129e5d439b09cd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

