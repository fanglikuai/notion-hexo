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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RG2BE5NY%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T030045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIEDz5g53aE3aFZMHZ5ox%2FCqppoN%2ByBIH7HuGaFWepUHdAiEAw4iUBBmaOBZlWwdyRErQoCED%2Bj2I966drJ2YA8w4ByYq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDKGo721q2pJjZxjHJircAw1Q5MCfLFKC7fpb8WjUNDE8zMrr%2FxASYkuLNF4bpdrBUOmoljCKZMHCcFHNmETy%2Ff45RtA4LglVpwzfRyybeZvQAeFUOUDh8WbH3LUm9dL%2F5S0GDmfOWv62yfGvRMmy8x3FZe%2FzrJfitrMWQDAUbAfMPK%2B9KR4cT%2FR2EI2itrQ7oPMI43rCpwfR6X068LMhf1gfYOPTsEVSunAE4u3ErCxkn%2FX8EhaGkfKh%2FDMrDkK2Ydek531OCJt%2FTqhbjGzhp3nQx%2FFCqRdaWw4Rm5iV6f0fovQiT3M4mKnkjO7eRe9NsplPbUOlL459tyNT0EZ7c8TFTgE7FTDEszJ%2Bc8n4%2FvgzQ7JwVbI83yPQRISZkjB4Kz%2FQpHwNDwmFWKvfAFRTinuCI92SM3ByIh675gr%2BM%2F1%2FhAe1yZCfNQu20Hfs%2F9Ly%2B1xllGg1SPgXVfnvoTKS6scPA5j1K4KLv1HmHJKD8w5SLaucKuMMLooX5ohbnXtcmDLigdA8dCdf6bc7wMYSHtjLY8rRuC%2BZ5v06G1eJFG5D%2FJOtoLXz8I81nq9lx%2FRn2Rb8uFcnj5yOwF%2BWqRjZGxnKVM%2BJ4B1NVGVbZWzXTKb4qpihColug9Bno10xlKiUTifzthVY9D59O76LMPOmq8cGOqUBMo195yrEZsbDcYh8nH7JBPusZ1Td1xDvM%2Fpjow24ZKobdj75H7WfFEGk2qIJc49m6n5MqARYc0%2B1nMj5haEAdEnhSEupU6jsz1LDXHguAxVcVpRPi5gXbRs4mLU2D9bnS097rEqQAC4Rr27ZVd28Ow2WvYP6hGWMhJCmdsJvWwolW%2FYM4AX%2FOruDKTmTXPIeG9Tt%2B2umQKbBhNNi1qk8secF7tw1&X-Amz-Signature=d13cbbbfbd009c78d37fcd953a44bbe57b3b132f06989a8b5f221ab54bd1e61a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

