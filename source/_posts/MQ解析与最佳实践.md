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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663TEP5YOQ%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T050044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDst0M%2FwsLZxb6nEr5UEHzJYL4hvdQ1d%2BJQzUHtyLbffAIhAIehHpFPM5mHUiwsvhl7qCLbSOnru4L2LJUUoTHbwk4rKv8DCD4QABoMNjM3NDIzMTgzODA1IgyTpG%2BeYXrXryms7qIq3ANJbNg2IyhcNXu6rFkDsfPaadJ1%2FY6ZpcMaV3kwNbgPARtWOiNDjEKqBUxdpTQciCWbdL%2BTxlsILafnYaCBdEIjSumQ%2BuCOQVRW1HVNqiA7%2BF8u3gl64EW9ukbm8oI8o5Vt3JlYt%2Bg%2F7Xb7q%2BMdpvzeLmF71ritO%2FmWAl3BXM0mMNFi9QFzMRtZsyBWZeMZrc2lVzj9LaAfEOHlsvFT5yj4Zl9N6h3ORavcIy%2FDnYfXWlkPGTvhC9OJIz0zlL1h%2BNiJXdJt%2BNMcIzMIo0rPS9WC6dhAOzFpCGtJld9Qqprvv3tlXUGOAVt32txguEkuYnxmlblwgyE85HQsCxtyv2Y5ThlXXvSmVyXgFwjjmFayCnGHM68rvOQ6M7EaNTUziGd9Z942A4zFCQ%2BxYx9pmRV3JdG38GLqBnznm88lRiXHVj8ueIGA0Hr%2BVTq%2FJIlHabw99gbuR88tbTuvVOIhpkCOe1u5Or5dS3byyKhPA6SBomW4TKIyXaAp9%2BwvSJVK4pu%2B1AAv3%2BleCHa8k%2Bzu4jmBmuPGhP358SaERsOM%2FDJrvBOTxHkFuFQXZsmNKPnZNctu98SICnYlu%2FcvTG%2BfFa0%2FSWcSvE%2FcxpZLCIcWvd6UES9I1%2BjZuRktzzvyMjCXrP3GBjqkAX31CzigmhpWZC7xiJdTviyZwT3qlWz27FFCT9ROA0pvI7VQYJ%2Bip38DcSE3VVtcNTKO%2BXqlkTj15VockyOJrqibKDYa%2FVgHqeReJHvqvXjLyG4p59pqpigbhVWGkC496SmdWlJ%2BBYj4W9DiIqj%2FqHQsUYE1vOuo4YbSH1SS4EmPNlYXppTz3eka9zetmVuWlICs3KpIVPatWbPAEkEyvRQOq75m&X-Amz-Signature=5a704e50ab9a65f5e1eb3393f16adb7055e740cf4962490b82d9fe6c85945358&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

