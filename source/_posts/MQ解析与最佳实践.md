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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663J2XJNVH%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEPOjhz7%2FV%2FxtZFkWFYJ8Cy4DRuFz8bxJp4ChJH6NCVmAiAWu5rYpieoI3ILVVvxY%2Bgg%2FYKeATKo2x4aZyOFZ6b9lCr%2FAwg8EAAaDDYzNzQyMzE4MzgwNSIMCGCKzuDzeYEWFeGqKtwDiXe6KAMX75GwgKHqWHYceEnwJav4R1VY1nP6XSHThsM3izh21QoLilcx8DgL9%2BVdZmlxfdgVwN%2FUoTy%2B9WdZPbKXOVFkf6tmfTGOYA%2BJCUCq4Rql%2FZ%2BIwKvg42vE9y%2BKG8b41UECFAlx9w7bWaXF%2F7xov7s9vMwyn1BPvzqjDW%2F68x0o3dMFjD5mhUJ4sM88jzysl6xSypKo%2FvVtyK5izopNC9O19eehsZOIdHWM0mmCJNrueaHGK%2FtQkB%2BYUX5DTW4C4cw4uvtB40xURSsTeyUV0EK%2FoWbfC%2FTJXg7Y5fAYrFaiPm1RvCtr4MlXjsex3MLhNO8%2B99Ag02wLxY01HHqpNTGNdZMfQ4TOcNZkiyu4nKeD2%2F5wfqxZ1NjNu4uNS7OKh3JbBZisx78ridZkecuEgWHp8D6f3FMQ2a5hrsBHh0wUGoJh96%2FvNoatxAoutjmaQ0evNqut6UoNaW1a98%2FjFgrrbrIrzINQj7k8PWAZ8HZ15%2FmCIvIcwHADF8s5s4UlGlmp2XrzlQ79hWQDf7HILPIAlnL4LbmeocYq3VGDIGybAKdgZIn2emwvYrugYdCshANnh6mUPvnHwhVY4mmsiTv%2FsyvvJQUvnQC3gDjUDUt%2BYl%2BQx0cKYZ8wj5LIxgY6pgG1Qd5Wi%2BA1wVSqcdk2w4NmqkUaw97aKrMcbVDb9OnT3H6i1sdtDHFhsJNf7WqdfFdWbbUXk2bOdUBl0vPGQ6BMlxS6y3oVG%2FkB2tShk1n5oj4pTTX9aoXAwagXoRNa7ktmh9jl0BGzgtvSQlgqSkDjFia3nrzqzmtMe%2FqHBeEotyz9TXvY4PcQUoOHlMZtZi2QMH2ea0iTpeccnNNZXovsJIZ53z2h&X-Amz-Signature=f7168c0aef52e3200c31b25f73fbe69c19014ab52dbc2cee8495ee8c2708feb9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

