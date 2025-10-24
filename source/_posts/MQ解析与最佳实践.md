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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SXW4EM4X%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T110046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDWNcDPAAUy3sXWCUctkM0h56NHEIH2LCqm28r3C9MSuAiAPfJZJFNFt0wgEZ38w0QjL2TRn8MWxW8kW4D76tQL8lSr%2FAwhbEAAaDDYzNzQyMzE4MzgwNSIMHrqFdpj1W8%2FQpES5KtwDThj5Oy6w5KIQwT7zktTWVEoRYOSNXPiKJ1Ziwp4L8XOpAoezVgGiWeNqwEukNIo2FzuU9IQ23eWUhKZ8FuibrVDvaUZtXEfR%2FmKPU9Y7Y1R9j6XgWO4qTzD8Dd6X3AGijumAVpF6hPhChanAlIfU86KGEf%2FCixtfpXUstd%2FBd7%2Fcj3ze5HxK561pd4M2pIwXo40LnXYX9Glero%2FZNLDRkEaz%2F%2FZh0d%2B5lHcUgr0wZIs2DbboJkwqRU8uMhMhGPicguDe3ypgMQHIbc9JgLPQwbfLTrVz1O7zLYezItCLR5QhlcPksk8ky3vyM3OH0PIEDt7pm5Uik48FePyPURiI3xmjXaiZG4peIbUjVCXCl%2FqQIO2Vt4dl2cUBfAvM88qC5wyQulx1bYgDrHiUnHHfgnSZ%2BHdWr7v1071%2Bksg8xDfS3si5kYFwIgBz9uviB3ujYLIz3L%2Ff10V%2Fjr9jCvV3M7LVxjp%2BeCpG3KDrXXI16f9SkiIO9kTK0y%2Fr%2FDFOdv1LMHYfiJCVUE0bNGGPv3NevOHvbAlJf6paK8kMxFzjy7PaEsOmw0omlcCSvm7niSk9I0SUsJDPLuLC07xhcdEvX07hNtIt4AZKByexCJYh9Ddt0Hau7VL0uXAWBCown5rtxwY6pgGA6e6quDo56gpyGvFZpbX5X47f7Fz9DKHhWIol0K7PgHaREIRSM81M%2FuFf%2BBLuXxnSagMKGxAa9luoGeNXzclirCDt0HqTu8iOljwqDv5MV5LWUACr21tDwsAyKvqdpJRoEH8R2uko9XMp69kyblTeHjjFbLR3o7VYyCXcK0kdDDW5br9cdkYi59Ft9WaLVy1Z7sa%2FeXKnRVguoJYAOgXLXz9pr1BZ&X-Amz-Signature=b2772071400b33cb1a2b09763e382083f663de3af0c3bba42725fdfe13afda1a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

