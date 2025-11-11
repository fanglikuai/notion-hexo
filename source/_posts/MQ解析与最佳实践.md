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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQQSCY6I%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T060055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJHMEUCIQDzo6LEJpWfBvtnc2V%2Bw5NtC2qyeE%2FIc%2Fs3PXZ7UAKjUgIgLJ6S65dAFCvZIx3U6bVPla0%2FM%2B9XkfriIwMpQmJbZ8Aq%2FwMIFhAAGgw2Mzc0MjMxODM4MDUiDErUmEyAXJDzYnuGryrcAxoSsnv9yZACjcFtKbBM1jPzzdQ33NSU2U%2BBzJYIMlem7zM2GhSR3BGgDW81aYz4evzsfCPDOnRq1L72YEjmP7w6J0e98kGGVJMRR%2FfMYi0UoMn0QtI2Fabj3UL4vF6%2B6AgiWI%2FKDzLrqb2XR91YmqGRAApU9TlgrHa8LUWgDPwjHhXa23Yeku3km3UAJzlHEHIDDlw8zsBjs7KnriQTFv8Ix73f6D7kW1AAlCPcvXQYNnn7LCYb0uW4GaBYxq71Cq1IYHqI15Etd5AyyHwEZ0U%2Fou4qlyuuowxmY%2Fz31XvK%2FbzuH8%2F5M%2BmAZzVgTg1s%2FJmRJ%2BP9Nx2%2BM7BKqOZftBl0aOL2y2AXqVeRWIzhhlUD58ncy0uzNKL44a52LMZgNIUoLTgNbfVD5SKHkB6Fg1dvdtj5eq7NhXQ8SW7pjH9UKbsmO8thP1r1fqme%2B271cTSKL2eOd9MUZv5B5MhZ1eZdtfQCeMQIFKrfvmldg9dIWPNXgIee1JiA%2B2n5ilp09Yr20HgbR3Z3eNICNqC9DjdWIww%2FWo6hkOp68qlizJDwfyJPLRWJfelXk2mP9VKfhAEcLMWjPh50ECPl2rOD2INMUsY2W76bX3KRu4UAVL2ceQIQ7v%2FAlnE%2FIDv6MLCEy8gGOqUBaxK8AUpkmzPN0MMlq8gCrvVOThVx%2BCLLKatJtK2jgMqaWjEf6Zg1Wx%2FWqA4tpBsD5C1fNk8Ft%2Bf%2BxhmoBNI%2FJXml4mDv9%2Fmc0SwjnUrFjhImg%2BupWr2oZrbsoeXwGbDuNjnUZOHis8chPH8UuYcjDZ33aC997OAyp3xFourzEDZGJxmg74mpGMzfl%2BM3omZ80uqMTRImNZ05Mg8jSVxGJeDz1Rl9&X-Amz-Signature=74040f8682a3a8118a334de1b4e9fb6d701ecc1aa78cd732a7f37cdc701c5ba3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

