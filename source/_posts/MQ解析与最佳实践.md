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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TBSWPF6B%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T180050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDDuPmSaF8wKT0Q450Y0xCoWhXvmWZFoCXZjESyl3XXtwIhAMf4zq1wvsCwMlUZNe%2FiWKvGE9qjWM0%2F7E02oK%2BTz49oKogECJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyqrMlxpkkq4j%2FScNcq3ANIgg%2F7dZRDsbzxRTPGQfuKlgk2Sn9D4ZmSJJDWiW329YRnId2ZqIbsSauq1iuwYOHiAbebrqGK8b0rO7RemBXysb%2FvS4p7hUzBxubaExxeMaGYpQbqtn1aCnlu0cLVsZcpmcrmin0oGqXB6q7I7VjDIYRIB9XHXp6emtr5hGzZMTfqG3Ur4bxXqk6qKLd6gIlZQW7pvf5izKSPxusqVxH795NGUYUVeegCWcX4OD1GDwGCqINLXNBUUi4iDR4vcrOGFYUu9gDo6NxAf0Vc9NjMw%2BNDVoFU%2BiQQSbe%2BPXmli4FeKLWGEIFsjcXuqwkDwCKWqp4zmb%2B1uwsBJKk8rK%2FuJZxxQitMjAmej1jqo19G7dcJAce%2Bm0S7GPM4o0PvG6qp0RgduYKgtol4fOzZp8L%2B9S2ubXF8DqlIIobmYVfxLra5qjq9yiGzK%2FdG%2Bx6OvltV2A2DxAZNMDI3bs0Uk7WNwOD7oUIwW4FAtcqeC%2FWft%2BPmBD0UsfhiwtU%2FtPW2Vjkym0wIKyVlAKbse57Ikgp6amJhnpEu7DZj0HSucRoKdHAxRJ8ttcB%2FNMYGruM%2B%2Blyr5OL6zjpNm7ISgyJdx%2FjjKil9XC5uXkW7ibP8m9PXDgKlt0QF67tWEmkFbjDt0MTHBjqkAfxd7aRQ6JGks3E%2BsxnqDcFQ3375i3NNF%2FuxhJUTnf0g8AHiReClw6JPxqXUvRsO5oZwn1P4AKi4WjRZXOy183mt7p42JyjNqdH7ZQ%2ByKYOGXP6ftFZCzwe7gUHzfxw6eQKkvn4sp17NtSfb7mYL%2BshQP9w5BMmBdCclhAipNku9mZspQxH2oNH2ooV%2Fsmku3RyoovLcziNtFX3DChAdWNbqqVAg&X-Amz-Signature=0d92b98583fe0235b04375634574307ed5512115448c1f3c87dde4cba2b8b52e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

