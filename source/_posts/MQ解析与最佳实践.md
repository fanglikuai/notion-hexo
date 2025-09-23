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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TSSW26K3%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T000048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCGC7wA5t%2FRSZNaz4ogID41E5%2BkSEAY2AiEv5Selm6ALAIgN6AMYEXB0i22L8EOViZv3cGTi%2F2EX05s0tHb%2BsV7YO8q%2FwMIOBAAGgw2Mzc0MjMxODM4MDUiDAcTSGl6ouc5ysdo7yrcA1yELaW5%2BsG4TM4yYTs9hMS0pCvpfVdgJVlOQU%2BfHstrCgZxiwGC3nINGdXhRYCTNxcsYvWnjMlH2Res6ZlfaGuw4g%2Bwye%2FDVOIdzncZ5feIN3p7cmcbvX1vor2fB5mV%2BmTDBah4tQ2uBBexns%2BXfe0DEC5UdQIOIfoZHQozv9rsKi6cKOg5efgdb2I0k4KQsgzyVdP%2BP5C%2BTY9IGTzi7NQFrUIhtVLE1V874HmtUnc7VjcXiyVQdr6TG0d249mdmVQpSW9ynBtMhFv2TCebW8XaQMs98Mf3ZU2JUpkel84CWDvM4D3xvq0rZgP%2F1nSsiv1SmCytmzC%2Fpaq4N%2BXKt4S8mdI8ljc%2FTGi4oNX7Hyrx3SkMdj3S1i2EHoJ3YGTGoWjDI%2FvLu815G6gSTWlRwzUWFzxQ92WNOXaFkTbo9mTg%2Fxpq2PIJjkLT4csgM94L34QeFSegDdXfD%2BSpNOD6Dk1uk5JBvyEQ4YUF%2F9zkuUEsLP2uycZwUj4%2B5qRbHqBUkAKUcNiKLF%2FT0gX292PUxq0E9%2FLZ7DPTQD%2BTy8U3AaZdT%2FexFXyv59aBmM%2BlukN3Ct4%2BRtDMGF%2F9vL393W9bhbPwKWU7geD%2BW8YE2Bu90hGr5XG5kVm7LYGuz2RFMN6ux8YGOqUBkosPBF%2FeFO9x92PILw5hsAWqTTqJwtfeRVPhboPoO5ff%2F4P5eWQytHAmO9WmnX1DkoApHJMGFis56WU1TH5ZrrZtN7t%2FgIxGZKPWvRd1a8RRk8462vpqYKAK7xjqA0sx6sVePVpR7XcKte%2FKVxjdPhHAnJQ3a%2BxIgJT%2BiirbQS1P8SuO1smxg8nO0uSTkxnIBg0ZzuX8qveMSpcrtCvHYSBQlqSW&X-Amz-Signature=75ab22cc343049da126f84632b8904fac2c6dbd0f5807788b67e4f09b80277f0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

