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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZOCLPEK%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T010056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIQCxeRPUmbkty5RIZ8qn1pJpvbZPnKV%2FGuhHJ%2FBOLPdSVAIgA30eFYkmJIsK4C1gzojsVQRAUCncsNEJ7oXMXX43r3IqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNGoKYgQM9HLCU6kFyrcA2rYd0GbGlSk5xHPxhwgeMaxtSDT63Y9ken9mLDyyURlha%2Fs40EwXk2WYnBUj1GRsvfd7xp7APKz%2Be1llAZHMXq%2FAU3lbIR%2B6ZWsHkP0tY6diDo3bT0fHu08nUf4Xe0%2FBOLpiCxvmISuIEyursfFhgLaNAeCM4ZUxid4%2BVvlhYjehoE3Pk2L3rJgAz1ZoGvw3U1ZTDfKC7cJQ0kOJAbVAr69n10gkTJUyfvbYkK17bsAQTMVHijzsKCoC%2Fa1IrVrRlnKVBZ2jAYMtKfY%2FpjF%2FazFFprZniQbNSMzacFe%2FhwoOx6Hh%2B0Cxxq15TanvyFj00jeNQ4GZsGLJg42wRtxn5UBKRPh6K68now5x4XD6188zNRuhTt%2BtQiEEYrnxVZtbzrGuJlEEIFnyoF%2Bg0O148uOwi9Pt9ubr%2BzXIcj5cIJBeoZSJVitRKf4NqnmeGHXUkr8ra%2FJ4Tlp98858EN9ip8N%2F1KOdhrs7y3uQ5VYBUquc7k5sP%2BJbO1PqOMrF1Dip%2BIOGg6IREDJY2faRqsNzqk3LVsZFSy1XO9Qy%2FAIsl1SiD%2F2azi5AQU0VSwbUiE8YJr%2Fwn%2BGV91ISBa5Jjx7TYCkUQW8X4gzsCSWACG7pdMi6FVbj0oDNJ1WjHVnMKz41ccGOqUBg6hOJlDTP3CXzAtzQaHud5qp8aVy6cEP3i9yJDfjsNhUzdlDDxuuw0z%2FdOFOwPlL2u5uBWZfHKFPIeJP3XlmeJbIWmXG%2FVYl7Bn2B3BJ5VWc87dcHEYEGsqYVJC7KixEMQCLy84QWA4fKib3%2Fbn4uAOwA2NDt0%2FuvBvo%2FplEAhcNaw5q%2BlW3cpOs5bNB%2FBepkaG1YyYSI4%2FqqC1vIOmz8t4dBFlT&X-Amz-Signature=b5a77b28a896d3deb3efd237790e24c1f61c963c3ba3ef2b792d4aa136747877&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

