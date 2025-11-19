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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SFIU7O6J%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T150056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBYaCXVzLXdlc3QtMiJHMEUCIBpyt4u46YyAiMkHBqqhOawA%2FevFO9m0KRk1mqrEe46%2FAiEAhlJKjPAt7p3txKfKAVWdo99hgzQKKYFqGaVbrf1ocrAqiAQI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBTYw5qtdZsw8G3EYSrcA2KRBZsnxoiWmN1I9g5K67mZOaMSCl26Ks4WbKGLlqNbpz8rJyYf7F9BemHoJFmeaP0oZr4%2F%2BxGdF8084B5m80Q11QFe7Di0uGc0OwOgPBsW4a0dqFQCI22nfn7Vz3PjusZj4ObaN2ShG8YGmrgAoIhTrTTSNIXC7I6l7xeHTVuJCQfvPWl12ornyB%2FtGCWg9pOgQWVDvwoGC315Geh54FeH1hsGpkqSlfi0s1Y%2F1okcKCbPBBqimesIn93mJhUbUlpBNo0lMvfSJb7M9B3HhKZy0rBPaHk8Lf%2BqdWvf5ozSCTpwknvfk72oUq%2FNjxLarR3dW4mq976xkEJSZyAu5891UmUnsca8mw%2F1%2FHJRNedTkWYf6X8HqY24tSYWqfr9m3ET1ritrcmnpvZySjWprCdSw8cz%2BqnnpBbqZzoDagwzHvfRmv2Z1Mmy3Us393p0YLE2buGJmzWGEpNeNsHIU3d7zn4mxmrF3jk%2BuGxlTJNw0csh3rtTIGuyd1S4bAqZi8VE5KIIYnzY1KRwCGcqNKE%2BuEv1GrCi9%2BUNBJheMPw6XtqaA2OpDxEHW60jZfCXX6NDHYmbk1MP6Wdnbi1BjCAP%2FFa3FHAzrsQ5OLwNoLT53AY2NreN4kG0hx0tMNeT98gGOqUB1EWWQzKNaut0kUo9byRe9AuIa1ecx3UWmsqBCXW6cWaEN%2BQ5rho8NS1YmlPHYywgfX4vLAeM5a3MFadqz6pm2%2F6ITMvLg2wmkgjFQQc7HX8VId2HbZcN9mvoEv9xSOZl%2BgDXoZALfi2oe7P78U84lOKpsdqYZboQdnUgHbCQrOdEM%2B471Unsj7pZ7xW6ToBWd%2FtoHOpGQnwSXR14Xg15B4CWdjqm&X-Amz-Signature=4b53602f7ed8e87a2ac8d22f9c699868f5939cf74ba3de69ea2fe8e1deab7df4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

