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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZE6PVUJR%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T110040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAcyYsRsIfYpby%2B6op4UMemLFXlSu3L3cFZczxwCO6bZAiBcd7zAyixGoQE5Xb2s4TQZn4c9cR7jwam26yDAls2yDiqIBAiM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMSpXDirPlyyjOlZGEKtwDSomERcEg5075cMKBmUWD6JapVZWaOieXk7OYQW8wpvs65H7xvToz6vvWrLwtcSrcE%2Bme1cAcEhjNfPhkVsgDaSY4hYKJYiHLs06dQzqAvg38y5uUrOXHI%2FjJTjgwn1W%2FlGvTaWnAzOPtzKKKHcAfmReldXvw1F5omB0yTo97fDcNwvze2TeIW%2BPF1xXMf4ArCL0zBxnk9E2744MgeRHIy2%2BPGZhFCO2aIcRZ1ZzuvcEeHl3VtlhyyLchGgNkqPs5qcuvnWwvoZf8WuXQ7QrDUPoStPrfzOk1x5UqPuYryU6xsCVFqhUiMj8Q1l4AAoF%2BdhohKildnhmT4SZZNSAbuRXlTlHjcJUT8sObYBU84FhtxwUCdVFsSu%2FzpKtkvqUppTTT4zqWy5Smxhlr7wAcxQaombnT%2Bh%2BhHaCQ5H49UEx%2FNqO4Wwz6n5HBAoQ6Oy8nPN%2BOMZMbRrfqzxqZ6uYQdg4ZXOMGCT0ZxzLgmXFobHRV7loXVj0UVOi%2FtihaA5Ka5cLzbHC3aSO9ZczZQhoBK1FwFsyeSEuILL%2BGFceWKq4lEPhVFEMGGHgL84QqLhjpH5NsCtwGP6p5pS12m7cH3X%2BI4LqPnMuMg2KB3wQJgzMx5vGyjdOs%2BiCg8kMwgs6syAY6pgEKRgT%2BVA5Fz%2BhmVnxZQXXgi9g%2BByCro%2BQfak%2F%2Fx%2Bsu5MDTeqkEcWJRQxcj3utX4a8CfqwHtmaYP2HgnDYcDmQ2jGL31mILrUsC0A1JHKnIaWF4XA%2BOglc0xaHZMm43W3C8gosvjpIQKia6fKlKtMSZt5SemdabMSEpoOZkiCel%2BKHlusgulgG1jRq%2F6SrPCPJYQeRi6kVXqg451HWxb2j8pfW3Q5S8&X-Amz-Signature=9ed074e4c73db8b13ff6826244943973866da69da99c251c4416c6b457e00cbd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

