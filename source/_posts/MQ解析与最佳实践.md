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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666WSBZ6E2%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T050038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB0aCXVzLXdlc3QtMiJHMEUCIEny1jsBG87kOYSO3GSyvY1ce1CiS94IWwMwy2u7pMyDAiEA4jDAMOrT6zcKCz3h5TdAiReBD8ZSAS4qIGPgVLsvk4EqiAQItf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOm8PqUZnc7uXMQYbSrcAw3SmhzpdcO4rNH1wv%2Fz0x1yYiJ%2FRm1x9SyToQplO4rHq8spAxh%2FGyTcrfrPZPZRa9d9qYKkC8e8TuXVmZeThM7lZhwSg%2B9YN3HVo2JUg16wGu%2Fv1K%2F70hIrm6XAfDBTB0PfxWexRT1YcsNTdfpK5tB9IFs%2B82xSYTbGnbj3ILVZ4%2FyASchQ08q0UOLRErKZTYps6TvfI5YZXAU2GUlX833vdck5A9xdvGJ4KOIMga8JW31NB7WcJIFnMj%2FujRtQVaaX6QWmxnhr1jwXnDqjp%2BD4R6m4vhbClW%2F5qv318aFjgytai5q9vKBbTP7snjEouYNPPQEw8sbz9hD1HIuqcKA1t88CccTwleChirNrpp0W30pti2yuVGtr43h8SjbdQg2fwe0Jj453jeYXkNt8x4bxc%2FijUGAEOelzbKTns5BDVC9CfobU5Mu%2FhXvzWNT7u6wXYeBEImHRlUfTe0NuAeNWthmEVsLtSXjsc98%2BqHeu1OxTybBQjktuf7H%2BeHLdEzsy7OBGQCTt5oby4YNcD9KPAc2yQZn5DZC2RSbLlNSfQE4wB9niu8gx7%2BnWHwE6QNF7fkNe7bqO0N6nWI5sfwBF4%2F4zZqOkTJ2jTtBzAx15MnIYw2phncwtvozGMKPOl8cGOqUB3Hp%2BXTmv2ux1%2FJHs1YDdSEBI1f%2FHr8ti9RZf%2FcEIAeGnAE119l1Ar1Irku2Ww2NRPoHR8HWc6KCNErXaEVJD9cQ%2BvxxEFiZrg5mpCODSwa9IS8wjo0D37tc9iLdkLEnGI3K5Dz9IMFp%2FbTM9LHiMQ6HM9CP%2FmqMlkwHSCU%2B%2FTBSq9WmUybmWRP5uuH2Zbfpj1w4v6Z0YcE73QGsnDU26OghUMRJf&X-Amz-Signature=247c4a92cd052d2d87beb8863286643852591a71505434d14a1a21e15f48d07f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

