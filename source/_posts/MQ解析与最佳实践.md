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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WWS7Y4V5%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGcaCXVzLXdlc3QtMiJHMEUCIQD%2BgBdTRo8nlRtZnCkl6x5L2HX%2BoqbPQqkDbgQpD9sGUwIgErgAlPfEqLLJKpgIlTbF9YUbThb9MhRH2ipgfLshKK0qiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCga%2BsN3hsZYFzEXFircA3rQwz4XnenZmhOrPfcDv1HyERaZY0AgKoANdA5wbV02FDAqXPfvCI%2FEKXIxxmfKqcAf0YojVXB3Ax7MKV0BljGaTdnhUbvuqDKqm4VMlvdg70KPULF8YKtYjzUL4TUd7ih6gDbPYSpmMOKi43LRAFOiRg4vesSM2Za5k77j9M7vVBNoV984roAVrvko86utwfz%2B%2FQR9s7F9ri35nA9QFY6NKBgBODd4yiEB8qo1PVCYSYg7kLV5Fpz%2B4ZFtWHMrBqzSzlCg9aElDntpIbKy7sP8lCklrG77VXUTSojbxENIYLKxoQCNr%2Brvn6eYr7fgT49mWzvZlhngYG7KQpCZ%2B8dGoa%2Fj0STZ5s1hZ5v9zjnatTvg65UfWcxoutWAworYQYZww23NfA3VijW%2FrV6z9XCUfjACRKLIDrctFC8kuE5JQZ9zGSHgfbSxgBhu23wBI6esx9ufnplzPMjroQJWR0%2F3sKkhN752s0Nn%2BTFTAA%2FFofpYoajlNPI1lZ6F%2BN47QyzoCKx7ok3EiJswKxYsgH%2FBZF4ZZvV%2FOGeOoCuR78a%2BYWJbyXnCytZNQzdsI%2BdOXeVv5VgvSDzNzSUeDY2frX3bbA0%2BpZormKHJ2P8JBjthPqE8g3zUDLOh6WmbMIrX78YGOqUBuxnuxoXrvbc0hXtSyzWmWUXHJ7O5ZDMlkVHQYUnl1nrp8qdg%2FZ9kK5PBHLhOPf3Vpm049H8hEDoX6zpSkWoakw1MTtb0fNdYHBy7hvhDjejCmWDqpbFJivkFgLrpHuSoWdf3hE7orivedKaueoP6bcJ%2Fx7zkD4VuD7tJJl16WFCmDMf35NJn1v1BYsX9tbag43dAWP%2B7Xidvamnq6%2Ba3UYJOjlxo&X-Amz-Signature=d4c7a09f84cad107ea72b225de5160b85170540374f373fba9d94ebdaffb3b33&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

