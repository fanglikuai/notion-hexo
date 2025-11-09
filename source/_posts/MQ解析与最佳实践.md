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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667KPL6U2T%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T160047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJIMEYCIQCr%2B1tqi62UTkF45fQj4cKmADta%2B5eUNxmoOzBjIVjr6gIhAId8r3dKNao5feqD24eN9JMBcW9LCNlLx7mKCLZurOTVKogECO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx9R4hi%2FVy1ykqHVE4q3AM2JPHOwYfRpgXhvEx%2Fgj2JDMVe7HLApTn4fLgn3%2F2xdqML1kYPYK6qfZ7ONxjCd%2Bv%2F2f%2FPWeM35D4uY8yqhghyCrT41INzyNZNjumzG9la8kah%2BTm7abruIDvdjOxmwt%2BRuWvoEtC7C00lqfYb9FpEj7%2BzKA4YeoLMdt%2BlBPeZ4%2BXCcXi4Y4ysH7ZrtCPus%2BYYg1TTejtFSwn6ywP9pr7hbfxrI741y7Avy%2BKkOyBPEjQylY%2BkI%2FzW6%2FUoeW3yYs%2Fu7dUEcmqk429HZkE4mWDHWo2t02nIKyMi%2F7C5XJTfPmgzlHzHgOsFDvUuUI6iwLleoUhhZdQvXaVHR6t%2B45Evk3CN%2B1onBwGc1BaZgU7gdy561UaMHMKvJm4JzgYZOewHGmw8FAMJxm1wDraG02eWn%2FmJzq2NQhA3iF60gmQhyB9NcInhTkjX56vVSaXfgL7iLInG31lvzIE%2FX2QB4L6YRTyAsSlvJnZvU6eSqUIdRBeJR7k4Rs60ukXndty3jjHAB18eaVUUaLt8NQhqjV8SwTspNtiCue22KikoRhMtoY6Pm5Y1Hu%2FLZJ5JRUZJzXzCQHnZHeM0DLJbtdXoxz%2Bnuq1WYDl4zncv2Yv%2FkFCAyoiw6BKUCpqQr%2B%2FFwjDUssLIBjqkAZplMxEZ14RV%2FsulJTL8jf%2BCTAzg0mowy99jkpiyjNNesX4byEZJ2LTIzYiH4c1y9EO1xGz2NixrG9BGi4wsxX09PWAKnFWqxjdB080v5DZmT3g5r89Jx%2F2JtBlK0ZzGwrjm%2B%2BJdKgJYdMd5Ob3MIx6WOGVr5Zen1n5OzN2r7yn%2Ft6Amwu8pH8bouwPUGaqvlIPSCMf8HzQYFdAK0cY0Yl5AO27s&X-Amz-Signature=f6ae421e6c343519c0bbbfbc573db19a509a1ba23d076fc9c9199b5bb914bbcd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

