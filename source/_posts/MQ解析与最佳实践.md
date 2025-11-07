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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WAJCFXQT%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T160051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBVCh4URjPNB3cxVBDzFpZNK4rfzmSS8FQ770jDvyzD8AiEAs3%2Bq9hM92L4sav7sU%2BCk6loHXTXI2NPzN8EMJr35SKcqiAQIwf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB%2F6iswO0Cs6FxUaMircAxTRcoBa4PucF1ljaQTOI4IgnTxtA3JXT7Zdp1ih4X6nBqCo82H81gqipip%2Fo2ZYLtxUXh2Hq7Oj0YgVCP%2BFSlXgTLFv96mHB5R3KvtMFRaNUAC7Zic7b40y6Rm%2B4CHd8VcasdXOx0dHmGiO7bOu5tlxp363Kv5w77Qhze7IeUTpRDPIq0TOOGc6enmuc2YdaF%2BEqmQNnDRvUbkss8yd4dCloSWkFfftjiu4P9Bwwbhw8x0rxoyVntjUMATnOMQkzbA3KUC7JlnXl%2FuclUc0zWa407fOekEhrOmX0DdaY%2BQJHp9Jc8Itwb9AmsdkOHbMuzZRnCoqKTXSoDarR7oZvq9pTOHG7t0JjDA2S%2BpwVcijkNNg5w4tUshGwOzOJHuzjlUlWplV%2Bp0Ws4DQezNfAe8VK6FpS4l2A0Z%2BPrqKOs7DkbWoYrF2iUi2pQNOFAEB2G9n9s%2FuJG8vzCWgGVIIklDDLYepUrg6cHWjSL%2Bx0ZHM2CnT0jnPGia%2FFleBFc%2BmUG5dtU3oltOncmB5MiDhl0DJFgOSHO1vshP2G7IUEZWNCUzXa8LQMZQOemmL91AiGmGDWYa0gapilb26SDhsxpiRj5RJi85L8CduKWslfphYZg4g%2BZP71iIbkPX%2FMJ6ouMgGOqUBdkO8LDFtBacqHGq5fgQf8NyKg36CutHyRkTc30FsvLdrOyZk1yM7UAJc1iS6qyVSfU6SVZlzN6D3h6RAwaQMTBXe%2Fx4br9umiSApKt92HX34OgoGLZXJN7A25QPl4o1eNQ5U8hkDnLUiZv5YQ6tmYMlYFA4nerOAoOd8A7Adwm73PU2gWejTzpctU8ew0qG3PqAGDaGDkjHlE7LejIcIbo%2FLqcaF&X-Amz-Signature=e3a8e0af29a65c72a5703e40372df7b1265b9e33e4afd615fcee1ef2ec8c0425&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

