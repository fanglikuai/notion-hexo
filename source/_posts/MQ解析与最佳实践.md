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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46645YQRRM6%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T040038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECQaCXVzLXdlc3QtMiJHMEUCIG3k6AROPt6wJyjn%2BeEkBo3qhiHZRgrhu%2FGRKc6meXKqAiEAhNar0DiODfMhQ3FTCZSDkDG36ktfE3XJlGFNwEQQzBcqiAQInP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJspIDfRBV5Ls%2BlM8SrcA9PyL5L6FsbGw1GU6ezLbJm8SIfwqQppAuDGRTpLxRWj%2FRYnaY16vR0G0knTtxoNRLfhKvOh1N42HTtjBTfY0zAVH8g%2FAZJruCg993YG9GV6skCqPegyHLcDDu%2F%2FpErE10WCz2voeHVN%2Fq1XnlTHeJwRLeM44kbECf2WuJ5p3BsBvLmFgkQQpEYjD7Vor83GmA8r0Hx6IX7GQDzawvg9Qd95Av8lA2tanQWccEggMFOsApcP1KruG31VLcFm0KbdKjNQ%2BXb%2FjdTMjYZsKMD3rMl3IRxgZ%2F6wPrAvwMd1M0RMUFN%2BDGhhn63smz9cnqjAKlp3fGRaDLs2TbV6kan3W2hbuVm92E7Ci9HxZBuYrqivvO0xlBVvZnbxEiMOd2Kv4pWabCmfeMWXKM6i%2FaRaR%2FTVeN2ZK3KwhQBiiU4DwzU4mjGVdRkNC6%2FVmrj57IkGr9p9RWQDrh3%2BWIP6WMS6DBzfD617xgQvGnM8W0OVVSD47rHuYRKX%2F8KeSJnizYs6RgzucB0SM0XVOvhq3CFBTluQYP50KOonvS0xwAfegL8XlcrL35RkYpll9BUtw006AwKXvsaQet6vpD6EOKdkfH55BYoyPhZBeVUeYnxBY1Qa5toRfceG5V37gGuvMKTSqMYGOqUBi8CW%2F9mP1omrX1vXnnd1WKq1JMik6OhCY22fUGyydP8I%2BTMJod3BwldW6A4xJec4P2Dh%2Bcy0effE1fOSnXX%2B%2FdB4ibcnHgHjmak9CipKqOu1kFw18gARXyeh97dDmPSrckEKGf20ctCf6gbKClJeSlhk4yyw6PBYVXNzeH4fo2dqMbxvjpERdHBcBLf5Eb2reGZtIIHcdKEaOqDspsVA58VToAT6&X-Amz-Signature=5e5f221e493159afaf82b4398bfd8d308381d2b8034e99dd17f3d8e803a09c0e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

