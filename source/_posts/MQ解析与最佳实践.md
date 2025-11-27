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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664FIE3ZDH%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T200043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD3SrOjU5J0RtDnlNebSZ%2B83SIyIu5wO1NQmp7QuXy3HQIgICn%2FGh90HIHPI7ss3hV11jmcpGaN%2FPas8lWltBH1uGUqiAQIpP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC38LCWbMw88nf1A9SrcA4lyjW0bVQ8ldBG4veA22c%2BJNSQH%2BKW0t14eVuWZx1rcf85%2BM7jxWvPn9V8kX2y%2BLBBTi7p%2Fy99EhnvhWOeinPwX6opy78a8SQCrCNb3RKAPaLPKL4IaaHUm6wZ%2FyNJww%2B2ExoyhOqg1ghjLi4xDnjJ6lLAWRDA6GOZBfT4DM0outjKDPrXyp9%2BcvxFU%2FpS2MRXsspFe3U%2Fxni%2BCOn%2FuYDRoZiJuBI1YtL6fk3wbINZqhtsjULQDqF7rixxiODdZbHdkbepOTjE8O9p9289TdMmb9qOXaVQFHw9S9g94z1SVypADIXqWuJT6QKRnIjis3E4OHljfVkc70kjf5zZepSH%2Fv%2FIxYBsBnzsqBwyFPnjZab1Up81rFTqLLw6VqZ4rbYTUYEzixB8Rx1ppTMo9wXpevKV59qjZeAJnUQVcGbrV5oghF8UsscftIFiFV4rvxlYMCtfrQ0sPzMkP6MbJ1O6byixXTI8NW6xT%2B5%2BHGShD%2FbARu9aG9JzGcGaFh%2FRO70vDHuDpqZWOGm91nh8agCax0ifSsrNBuSrmakhopgrKOdfjPxeF3CXStwaNn32bnM%2BOedLBEsiSbxTw6wzwWhJcGKKzbmUlpo5nMGNevGiQPQJRU0rnCCVRVT57MMHFoskGOqUBcnEJgOwj0Xdfo9SKHW5%2FJncG2bcdydlVKQUkEg9NaN1V0NbxFUx9gVNcX2vPSTNxNuKoKZtEQvfSoES%2FFPp0ZboaSfV7l5JnDlXWPtYtdIIk4GRjH%2BErAUwOv73Mk7LQI1CObN9rLwVGdPCm0vLkTfQFiIJzR8YGGCs2OASYnVq7MiuRbaNZpzgfcvYkufijuqwCZ6eJQhUJU90ZeC9ERgSbf8IN&X-Amz-Signature=64fc2f8214f155ca29b43574308684a44ddfa5853846160c1d990d6cf0f61bee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

