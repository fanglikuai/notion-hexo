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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665PTRUCWX%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T070109Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJIMEYCIQCIM6D8dhI1upCI6%2FZu%2BNikzUbfUUjMalwOESJBhJdt2QIhAKkLoxbYBMzwssgzuLVFGrYapZ6%2FmYtJFrqQYkPH%2BVCUKogECM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwRKDcsVS46bozEd%2F0q3AN9DJtQgUobf%2BiM1%2BY1o1P05Hrj5DdghkIHCYGtOknAOciKdC0KneY5S9%2FH1u9LLo8YZJ74JH3xPIQ99g9Jgra%2FFrVVYYht76ncMj7w%2BCS6nSDcbyHjHnFzIjbkQ0%2FdFXPWwxpktqiC6lvUNbRKNjUVnsaYbzqBo3meHDz6J8e9AfALuoFm%2FWp%2Bu32j7yLXsQICz7H3EBMtw7PR2M%2FbxKbURAOcWp4pDVUGUEyv9b3vhsMaJnc60zR6dAs0biwnbMXIBP3VW33a3m16UCWmE4nsQugh0580X94Ay3kKX3d%2FPqi0RBt23WnvlDe%2F76fTgox%2FoZp0QHXfaR4dZYBoaf4N%2F6QYaOohSwJP6p%2By5u%2BTN%2Bpe7CoAqFNTfjvqdvQOQzGDcu8qZeblfUwFC4tt2RR4uEwjAZ4Sb%2B%2F5%2BNnWcJDFtOIb1vmIj5gUj%2BAgqY%2FZ4dECBY%2FzJehk5vEpObcA5drP8X%2B34xHgUjNtwmxP6JkR%2BmIKZ5DoX1XWKUPe6bgJWEZw3fnNRX7GCEFHPCAt9MZAJPcV%2FEgJ3KOCj9Ni7VQmaNfQKwKk0TlARyjomqvGEBUYHc%2FgythWsx66s6kn4YDrPPTRxALOpvQz1wgIRvqvDjzXyypYKlt9eD2EvjC2nJ3HBjqkASiorV73D%2B%2BTfk45lPUToTZwPYMvgWVdihXxfRb5FaY7m0FkHdjPOhALQqC5%2FvOJvGMppnGsgiV42m0wVHRB%2B%2FOvNkVXUSO96xL53ilkpkc9Q0mYLDt8ABbBsKKUx6R8jcIgXMONJNR9KZMRG3mmPeqMNzPCzE1iuo7qIbWZr%2B0ieThvlLw4DRUpDPo5jQ84VxG59%2FOxXfI0XyTjJBvBmRtSThnb&X-Amz-Signature=209f5db1a11fd896b8e6dde58a9973a484a7644eefef8b7a5f3f5601a35b5c85&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

