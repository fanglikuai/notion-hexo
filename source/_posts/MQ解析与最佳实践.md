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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46675XC55U6%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T030114Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDIaCXVzLXdlc3QtMiJGMEQCIAYppzUCUPlwa9idFGdoi9oUUryUmh0bC8SMA801oOVEAiBtsg1dhbiyXBXL%2BExIRR5cvSqtZAYpQGy1jgLk5lYWGyqIBAjL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMjtWg%2Ba4oFBSXky6iKtwDIF%2F4aB3S8rzHiSNFaYmE86ruB4YfzerYaRGR1Bl6AM%2BsYPfgR4iBQphljWMxiA0HJm5a%2FxAZRUUjr4MqPuFdqWor1giR2QMwbUudKvKea4QoIIDR9q2QbWBucxexm9yUJ8E%2F%2B2EIrOYgwqqj44hyphHAPCILAH6GcaQgk3D2xX1QuRg305kD6PwVFVkGkr82G6uaDG%2FJ%2BUyX0L8YxdV05eXb8Vkcy%2BX0bMSJQoRqL1qvr13YR40BhEK1Omwa%2Fj1rH0dolmnu%2F3XcibMQxsvj5twmoj8iOVqmXxxGcHmnpYwho9ppU2OXrZ52Cs5W3h4FhfpyDGBMCPMYOwonJxernVj8NTK8FaYYot9VLN8V8d6nK5g23aHDci2omxhC1PlfQ7X8aUZn%2BFFYqsBLtbYz6C0VcEqvJFO18qX9IvquPboP7IsDpeduTEoh12p1CIxShDOCbO7HhW5iUsUXvZTWs3I7WvAbAhscJO6DOZTbiO%2BaXYhdnyFCx2GoKUnt4vLQxfzuS9xwMFaeE1RP935g0MYH9mG%2FPCKwVeEuGjKpy9zAQM2umuRJy2KKHONjhGcfxNyWDkwVbY3Q7W2CjCckphK4wXn7sggUAGacVeOgeTg9E3%2F6oKUIt0rigLUwhqicxwY6pgGCrXh0T1qgXOsCqxirQ8Bq8AjB9CGylormeiprZ%2B5CeiDVfKq7%2Bo393GXow5V7jM5dXkXMWSBb7PoHbwq5uj8aFz9CSZucJT3DvvU3JiKbVw%2FoJ34sCUbffwmIcgZqDUKWTU3cC5I2whoLZ6Ti0meMXFfuYjLeOYUrc7tDT7xvKtTDjgIiuajG5kwKaNah7Yr3U%2Bmoyv1C5azIhmUmR2%2Bso2p3i1%2BQ&X-Amz-Signature=11014155555d21f22006c52807b5e2b6f9b9891b7407be7209a5620c7703a6d6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

