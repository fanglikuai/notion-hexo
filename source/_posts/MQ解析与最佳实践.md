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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46672DOM3S2%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T030057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDe9uZR62RX%2BYz3pS3kzbTwCH9gP07V7MZeDWrxyEM1DgIgYMNvecYNC%2F2J64BawkAuHfhi4k1zzznks458dW7Ehdwq%2FwMIOxAAGgw2Mzc0MjMxODM4MDUiDAzaFTkDV35M4jCLMircA9Wkdk%2BWAeIxpKNQHyeo7HG945mHlGI2NAbh9z8Gy5uGldiFTbB7Db71mXfC2xCZFQzf3%2BfcJpEBH6ReEJR4ed15DQP%2BYVlbLwzr5qfpsFiwCchLf5DBIlQQzuL8iiLntgM2Q6yvspR4ctSJGGvsEGwyClkXriCMKTDXSPLYLBchvbpWtEYrqHzhNknz5vyhjnaHrA%2FL3ZJw5TjYsBFmbE%2BwoRCeXNjAOUSFysjBLZ34At4I5ly%2B9wc4k6tfcx9y%2Fwo0DsHhkwpp%2FvLWSMOTJNJXfK1Cp%2B3Rk8bd7Yh%2Fc54wfjZjDk3GfoiALrpgsqsPm1jAuH321gC7WOju8COFiZCEWkSSsv6wNQR%2Fkju8XEYJRLWe4HlTMfi2Ic2X5TdZYwEXlDlteidR4qI8CHArP4WQ61K4nGBFxD4nmfvFeXKVbDu%2BMm9ss4DI%2BcApRin6BgyF1BWTq1TNWaXw321n0jagMyiNFroKT1ih7XBufciKVxXvqzlRAcEAasclwGZvCUypbvfKMUCvCda5Nnrh2Xcp57i78oOd1cYzGPEyHaOUbuOITgaBfnnbMwuvJ2ZB0X%2BQY%2FkdirankmOAPNDZZc5uR%2FjUdyC%2Bo59WW%2FBZ3nuYLcob5o6r9NLx5TpOMPyd5scGOqUBARX4KiLLFuEiiHOjX9D0KGSAVt4no9LzmjZE%2BPUjI5RzFjKUT%2F6670ql1ucCQh58P3z4%2FiWxX%2BdbYV8xEBOHwjzVRY8Gi6hl517EiTHhCmXHTIZOBAKqu3wGXnA%2FlgVomh3%2BCHTcBfNU5dRzNXjpeoJFUJtgdLs2VFn5h6qhfeBxE1QYlKWuNUVleWDpkiELBEVTkocdFCQvNRPhDBWPR6hAGT0D&X-Amz-Signature=25783856fc2dfe18d3eeeff7df69ea9c55e65fc1865704c97432331be3ce20f0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

