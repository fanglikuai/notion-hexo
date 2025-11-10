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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVBV7RUQ%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T150058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJGMEQCIFjzX%2FGyjZJrUumyexRcvduYLe%2FPuaJff3hK3GwGixsaAiATs4FPtOWMCQnkwQQRnq%2BG7OdO9t4gQ%2FtzIPoldmXuASr%2FAwgIEAAaDDYzNzQyMzE4MzgwNSIMUYoTXV6rMEMECNaDKtwDwRYVoTEtS%2FsHtoGNKqji%2FHNBWLbFW0Yx0aJBc20RpLp0z%2BbujVdn39H9FO7RX4AF25O4zEy4nuuKoqE4Zsn7tHrv%2FzdEx1o3Xs9SHZ8YFzuf84MgHIKGdndjbDisR7r6j4KBJV4OLkMfbj3y7XaD0bq5B6LbALOWZ3hf%2BEXML1pQKGuPr5BlqjgyA%2BZnY3jDG6H%2Fnaqccdnumvqvv2EP7Kwzjzs6aW0aBytJLcOPm90ySjPcmzJvvg2O3VLMsB1touKzrx6%2F%2FwU%2FabDQi%2FbLDOkd8fZptfmPQ9yD%2FQb503BuQsWzPGZwYU%2FTkT3p1rooUzG8GHIwaZymd5zetwo1%2BpsjYn%2FLbUkM3bNo0Sx8wgxBBcOLtdZwA%2BW1%2BW37Qw1pP3J78oJ9RY8%2B8E98L64zhfxfvejNLqtS697zYOC3grm461HzhvBD%2Fdatdnbl9ixazSkTzgVF1E%2F7VzMoBG%2Bfn65kCjm7UzNjsumr46AZE%2FfHvl32GO7qwMm7J%2BH8MOn9WyAJS8vO9EbxRxVp7QTpSTHJqn9deXHYV057rCZdJqCL87iPmqjw67bczdpDQVhRSumvBmUI5fcJjxMsA6ICgFvkwHlwB64AkqCbmDzkLQMd061ZBIZAwmabGQcwoezHyAY6pgG0PeQ7En6wwmIabNWRIi1ycZ392kodX4q1jJ7UoHIls09cES8Nc8c6JB5XkENNTyW8xuH80gv%2FldEfMv1oHAsqY0lK5FOJgXlK0chT4lTd9NEO4D8tsFynMWxEPRXeil%2BCWUhspZ1BHg6%2BaFnv2dx3zgsmi%2FTG40YV0xRv5k8rSxZutVRTzKoXRnUD6cT4gbbol%2FquLUQd7m2yTeOmeguwDpy0d2Xu&X-Amz-Signature=22ce2eeb8621362e9b5f9fc7ddcd43bb413a6c32705e11b371d22b5f55cb4f59&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

