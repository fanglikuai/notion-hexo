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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SWCZGCHI%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T110049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE9aO68e4KR635XSJQ7Bvf8ABJIuNFF2MWIePVjvteW7AiBv2NUZX9GkrcWbe38FmqgmMQbg0AWU8JgrZ3Kj2vXbtSr%2FAwh0EAAaDDYzNzQyMzE4MzgwNSIMCTuGtOvBjSQS20lKKtwDCVLNGdB5pvFMYfDUqDbM%2FubD7urnCeh3%2Fpt%2Bj4z7DwBZS00lk1KMYjKt7mYWP1Bvr4gAoI4MPRqyikEKLEpMwlHeFBYVqaAHRgw8MvveC8rYMV05sem6QgaepZzoRPnTkjuTzoZggX%2BkKabIoQtm%2Byd3VTbNulZb1GtNOZKckeYUGiYy2kUUI6n%2F2Qdahdk%2FLXuIHrULbUOgfHGQLDkl7hCCTIuQGdFUAcNWS%2BRkUepHZrrG05GuzdGlQ2xPs0dEPjQKXfayx9Kc4vso37xCn%2FKEBb8a9qpIT0j6iwBYzEwGe0Kbk1PDbTcwGIXfvFjXgSgmFES3AOH%2BIIYuHQJ3oLMz97eAw8Qm1DKhUNbYIWWyPmbbD8XfNzmRTHem58%2Fx2chDwltknLdqWNj%2B3fja9oukbvGRzxJPCe0FeI4YO%2F1LiCvRzXtjrudcrOctRy03DpkVEmYfCh5ll59CwA0K4T2BsCHFBh%2B4DhlavPDirmQTwNQB%2Bx83VoP4cPlgICt52%2F1WE4wLQ3tj%2B22xz2gNhZkm7Y5qOGWRfYKyLFy9s9Dl2P3rtLBkNym8De%2BLZPfMah5sOvS%2FmUNqDzaZq4TcO7pM6EKlWSbJcgNW22LbLh8kPq%2FUdTJi1VZ7xygwytfyxwY6pgHBdvgObfqxEartDORaNVWZWSszTBY%2B8IpCE81b6zP90XWp5CUG2MzQvDe5PZ2iiR0U80f5HdkIVRcYY0Q1ASYZrNdRdf%2BA0DTOZKmWXn9CyjbZwcYDU3HYEAhsKQz%2BYUOdrqSsxywzQh5a%2FufmCjOQK4aSTY5eHq6XyM220gIbLvjlnkAjKPm7HnwzeTDr0LAelUmbXvH4hzn2DeZc83pH2tdXyPIW&X-Amz-Signature=88627e87cc667d7f57db19705021d3f184300d08f87d9022a5f38002c4e41d94&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

