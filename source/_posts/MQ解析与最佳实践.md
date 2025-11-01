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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QCKJEAY5%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJGMEQCIHzjd7A7H0wO8S5Jgi0Q0Z6og3uvQKavfEWUnybZc1QsAiAQNIdZ53izto2kpI%2Fnxqh4CaX%2BvLFBcxocwJuu6DV%2Byir%2FAwgkEAAaDDYzNzQyMzE4MzgwNSIMuDm7JaII46DoJ54UKtwDOu4JA%2FlxDlSa1lvO5Cg6dcVrlRRYW0Xu5U9UFQ74CC8O9ezwg%2FTgbJCIZRcEP5hOg6LB7z6mPInVGkvkDpntujAZZCDvPzYUWQcjhOrG6izxtx7xpmBrT%2FlHFHpqZAkzl4KIg47lKhCUAXSXU4q%2FW5fqMj8HMvT1gvriE0gvFgqkNhqvSqbA3yJZbE8nze1enaOBFhOh8LjvF8Ne90zzUxwn6eOZtqdVCzhDKNXQ8LTtlOsDYw7Ioa3ZL0QtZv8BRN6EQR8n1moqHSstKhRVvRK4yPy1XRGtFlD399EIMZ5A6dQl4adIGgK7jYef74EYR%2Byivb8prbiKiEYDBt7sc67PRs5hoCwxI3YMNKRJ1cxM8aTFUBdHN4qzWbqqS%2B8hCiVvHfyB73nD7qky62lRw4DBs92EivO4lrwp31gw4NaiWRV9lD6OMSRLw4f509HcTrA2CgWA7aW%2Fm44QLMuwotI%2BQJ1clKjD6QrQowvXYK1qDSoWkGlq0wE%2Fqo%2B1%2Fljm7bZEu2i0bj1s8cl3JEtbatteqhwXrjybRdi08Vr86TQsLye5ivZ8kAYEF171tU3LqtmX%2FldRWx%2FblLlcHs%2BbzvdjCMRHePW9REgbe7qJ%2Bl2OhEddHxrSh4%2F6ZNkwoOiVyAY6pgERXxYmf%2Bz0yz7xJ1HNtJwleFZHnqg9sYUappCR6BLPRRFGPa5FKvJOViYOBpYVGv4WItrugYRLlWMqedktTcVS7hmfE7povpUTcYfrB%2Frneaip9aeVz498ZMWPA0CdTXLtQw74ctuWLlRZQno5kRS3hFaMqzEQxmym%2Fq041bdLkMf9oK3dONCqN51lbrvnBia2JQNnfPAhYmoK85E5396K%2B1tn%2Fbkk&X-Amz-Signature=5afc475cb4b6e2a70f89a5ca9c9214eb50c0bf82fd4eff538229565ac2e654fb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

