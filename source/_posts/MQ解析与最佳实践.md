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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V4T47E2Y%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDHTRdVyRUP8RL%2BziDHj05dcM68qUR8UUEaYlzczI0baAIgOABzNWJrLb%2Bc6J3WJ8TI6ya%2BpsWiCB1owN%2F12f2vVygq%2FwMIdBAAGgw2Mzc0MjMxODM4MDUiDDsUtCJE6HtUcjfAfCrcA%2FPlOHLAg071sjzqOPazZBPVtdoCeb7ywsQQrXRod5pzTD98SJWioPbd8GxrQTkvEFTjdNZHuTTEndBpEI2pa0cUHqIVaS7dMr%2B0MSd0cE9Qygl1h0HgyM%2BCKCGRXR00n%2F0hS8tiuP2viQuhtYn24fODR0SlxyPRKUX%2FNvRhm1l59epjaYrGyTUj%2Byo1Va%2BDj2d7fPMtwGG1w5y36TvVZT8yaOmvZDQQzVGtKY4JI2n3ShNgwfqG83Xk56MQKdJUKJOCfwjwckXvKcZw8wWMUaz4MXrClfSYPHljZ5f7JcirdoHFOH02XmOLpCNIDylcd5GB3Bu189uRQhUpkHouCTcck1fQ0DS1WIgxD9q0B2TZ2g%2B9I1zQf9n2IffZzzjMBIc%2BKPI4kYf0RRu6ugjGJRBSsqFbas32WVu39O%2BcL%2Bt7hABA6wNM9ixC3QZNQEv599SN981UAkPhsS7F7l%2FAlzseYZRZ3EKukGnYiWu%2BAddxIaIBpJU7vhbeBHllXgwVaHx8azIIUTOVaa4db5ZMrY6fP1HLYwVGKdThyWCbmv3764H96paY2zfpoJHhCDV%2FQyv5L%2F5mZNJPHVnIchJzEJL%2FIwgjkctgjE%2Fd1qpFI078Mc8HvCElJtVKCLX7MLq91MYGOqUBSeNyg%2BsUTGhHWjAp6OfJtW0%2BZIaFIftwD1RRRMvDCVqcMzXq2iwPHIQ5kR%2Fy%2FioDhdWOO%2B68ht5wj5EO01OH6wJXlDM7eKRRJpsIIAoUHt9oxaF%2FhBhnniOe4iqFLZ2NytktlyIYOCpOkhATi9F9DFwXw635bwijK5zac0bUrLz1gfz8q64L%2FHj4CPdEkRrUK7A5iQK3ozF7MQV7R0QJ%2FTYteYR3&X-Amz-Signature=cd88d6880edc83b1deb4f616c4c95b814f117701feb589acc366d174153d7e3e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

