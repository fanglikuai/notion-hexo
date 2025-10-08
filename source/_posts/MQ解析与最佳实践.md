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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U3YA7BDQ%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T170041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECgaCXVzLXdlc3QtMiJIMEYCIQDPK9BC1e5Oh4LjRiIUZNHDv6HpAnw3yQleor5%2BCvj3JQIhAIo1WreiJbmHOvs4o%2FoKHd7TUFzPr21%2B2RoUn7N2TtpHKogECMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzeZODFcmMi61jfJeoq3APasbEqmP8GmUkWF%2F%2BxkJt6SvWStWp2YzSHXH596iIJIrTBjVjnVFKGzK2xiiHjjGGb8%2BLbAgDxZAWqFxIqWtDXRMiV0LfdbcqZcH2u4j2v4SRvvWXzKuu3P25ar%2F736xd3bmpJs%2F1xz9jbPhgJdbmP8F%2FqL9xjtFHR79JB51Vv3MQGVcVVEetiywTQVPmB1WIEwUbynzygK8K6ITuBV4X5ddNAIZpi2e7VJAbuL4irghd5DNxklf5DTdo3TVr52wW988fqM539uiUcAbmAN1DbKDnl%2F%2F76PYzaoQ7TrhbzWX5d%2BSRiFMzJ6yi8WPf9x5B2Z1lTVlCOINhXj51WX1PKud4zhzcT5%2BhkkiH9FkagCBNDbNuflU7ZmWmIrF8FVf2ED3FaXMSzOR5jnSueoe333GYMQVdTFwjRp%2FvwmAbuFSl%2FpAsD%2F9X8Dbzy141ROufJOvC9m0cERJJokCS%2BiuyfandIiZAK5aIEcEi3qVcRLfgpqGOi4IJL5cBKRYDXHtE6Yf7hnQuuAWQTr5wLFpJCRp4gVt6KtqiMrJA7SIuouRhNKQ5QB83pXEdRxtcNH2ti8VWKEjTNvxDJlte07K%2Bb%2FEJVK0izhaJURcE%2BcfVsZoYApfv8seOKmrUzOTDOnJrHBjqkAad37dy894Ixpc4zihwkT5V6owvDreaGkeHI3RCzW%2Bnphd%2FtIPyCyQHpOsGr0Pa0eFB1niIQdYLtk4r9XLqakqCLRILgjXa28gAyheQ35yrbLoWBU1yFcT1%2FWPNDKDYNgQR0zlBNlqeC8z5qRXoIU54kyyFhJGLGUFcS1nKMzQR9tgGludafwN%2BuPC2SnTbfesSvNVpu0UEX6xN5XIt3hiTpbz15&X-Amz-Signature=831474beaa43e35706349f378868ed23c9937bd339a2a52de3db0acde5adffbf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

