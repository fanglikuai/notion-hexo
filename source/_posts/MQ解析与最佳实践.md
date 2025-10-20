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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U36DDQW6%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T190045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJHMEUCIH9pxv%2FVqRkKWNL7%2B7CjUBNsLAdnILHePfo4gsKJuK9pAiEAoTpZpRG4SNw4h9cDQiw5aThfRS6%2FnJ7ICm0JeOzvK2UqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNfQQhTp4YhzNfNwaircA8BPtPggnuM8o%2FNgQgMYKmJd8zE1%2BH49%2FOy%2Fv%2BF65YyWP29bEoK5wDxnSCwYP3scln29uWU4RByrimX5vDoUB2Hc3puUThXYHEPthcC8CspSmBx7uK0bZAobCCIV3j0ZgBm5C1FdXzjovqZ%2BIoow2FJpf%2FO40eZrEa84zzvLvElS89OwOsaANounzPbLRTNQwHiBSsri0Ze3E3bRRv9TQgArSX5wEK%2FXVR6XiXrNNdGDh3Upk4XWhIdYNCwDsmjVmPdaopUHKAfJaT0O64QEoZQ24I5kvlNpdyR2SpBDz0MtZlgD6iityyu0ArXublZpSM87gMOq9ETuK1NiyO1iKFO69jY5PSBT2OsZi5rfVlNwTDZrAsT1uTK%2BjHgwcyjd8qFWIuDLbQY1K%2Fd4kGS8VS9UP5FYZlSyuhJvjS5xrkIsLK1RtAMyQkxn1HBM0cUYb32hFB4ujpcGbA3L1uVD6AbK1OqggHb2aZUQ9U5Uv2kNORjE04DqNRMSQXzdXdEoM9hBQ%2FpJF7eaPWJ7rVqjSgCi4nvNW9Ob38cxmAoSu5ysP7G1a4sQqTs%2BSNB5oGwjCAcALMFTUcGdTMewyYfLRF5I56VKWX9YdX8o6VH%2FZMyFYtsur86FtwlEkMKoMMC22ccGOqUB%2FVWNmUuJ4NyuRqgLw%2FtiugYif6eVG%2BYv53MNxnRbOrofkeYDIVgef0lyjHdDL7GiHPnQ7bJTVBA2QbKn7iQEZ1QqluzyXSv51dYyhv6GCvr1ojHVIGPukOWMw%2BL%2B71a%2FdQMhzxD2oQBVUXY35Nk46lrqLJX5DdI5TZmGRq4jOqbAEFeQjKalz%2BENoGFT4scJYvarq1n4WoIfyuNvKlGzaiOzcIPJ&X-Amz-Signature=b997f0e1c9081a0e849d6299ca7ab579ac02bb84ab7e0e67c4369a5644b3c108&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

