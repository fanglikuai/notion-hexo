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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UGZTP2Z7%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T220136Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAbHKxsodESw4xdyahhnTlpSuB%2B1Qlw9zMnmfBQrWnJ6AiBSzjlcXLoAvHDJJBjyrpPwiC%2F86CO27ZkJl%2BOs8%2F2bpyqIBAiu%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMuRk9IIqIX0OzfNNSKtwDXD7KbGpPbcSCdMvL7MgEzt%2FSkvQ%2B5DmnhQBqOJIMA4gSgUTaDGmn7IkXrFiNEiPxGBCxiktZsSgikHiTcZRNlKK8P7SJ58nbiuVgPG6xn92w5tcvlK33r729KE3E7gh9AyccRPp31ObaLuuldCyBvn7oT18QiIvUu3upTKFILC0ZGdfHG7Lvr3m4iKTjmiiBwF0NsvxL1RSgoXfGJ5m6NVu1Lrq0E%2FZouiat8MMoJ8oEn%2FIuOPWh94oUsj18YGUo7yUke0BxnWLZOnHOlWSUYXU%2FQmqs00xpPIi%2BjE%2FoCXVo%2FPy%2Fr4gqhyDZCtpJpEiZBB1WngM%2B3c6Qsw2XPDWk%2Fu4ge5SDDRI1WCUt%2F04icc3raKyyTrAx%2BVUuz16DnA%2BI4GtHkzNpqSG8%2FKWMf%2FwGEX9e%2FBsZ16bgbfYnqRmRJUR25Clcjk23nv%2F8QjbGpnnbOcj9gxAG0oIhJ4XhndXZCLJt8JX%2FcpZ5D5RV8dUw2GZMjCDvyN5iic24Oc81NqQPjVAchYN9kQCFB4ZxW2KjeX0OFZHyojRcK6lWzPMewsGGcXMttE0xGHDTZ4gRtS4kck8iRcin0U1sNjiC0A7PFHQB%2FL0zLdwAzlHjIUTPmi%2BjJr0gGROv0CwrepMwjLv%2FxwY6pgHTsdNQ17im8Ofu0mZDbxc%2FPPUdTXitmMKfjucEJ7XL49ojn%2FfgzYrgUbQBsetovII4PLoAavzQMpw3mloFPkn1FfkDJ1E3jcP7fKcNwsZm65tF6RlnhGGiQJV4a4yzoGEeicjqTlqnxOdQIwdfLRIX8Wg0Azyw1ePMXLj59ADWrDXmRjOTH2GLnbRYmpXhbBkqVML%2B5e66rK8KZAfAM02kRwyZ3W8K&X-Amz-Signature=3b4936f4754f1a47b275a426a215bfb81a409ac07d50a89243dc1941a824293c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

