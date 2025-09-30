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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RRZLCGJN%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T060044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJHMEUCIQCCYkNXttw5RWHJ3OBr4TbpGxD8GO5PGsNmswbIdgrQpgIgRmx%2Fr7HMZpzZgGCYZdKPdL0UpwXSopiMCW0gQVIj7xoqiAQI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKrKDgCAVMzNGI5DVCrcAwcANL9myXWY2e%2BU7h18SQCrmGVfdgbDFq6LVFE9EINnEb%2B2RT0AAKfrrsPW6X6YkCbqp%2F4MOR9dpoO2%2BTBL%2FmodeK%2BRegMnPu6UWY%2FExAKo0p7HLbVZGUnMNeJ5WrUvrQ1WzmPj4%2F%2BLhpy9yRxAS01S0oNOqou%2B3WAFon3KD4LE6Dzu%2FuupP1ZivbXobFdOgVzAja3YCg1%2F25WO5GmUyEqHE9n67tSaMrXckVNBID91Gyo31mhyDGZi2DPV%2FzCNut2SAoFU4PM07GdbW%2Bo0mh5lhb3YAysMPzcgx%2BJ6ofR9R7lcLwCymMn89Wx3LO26nMDWk5D4EwnV%2BElz5RDjBZe%2FuFGbwNPrGF6r7m22l48gMydyaird2RvGYlk3yPxhbF7SXrYlrC48LuPRH8fmxAnJSHW9KSxAY34ewe0T4WIRVbBWdjWZnnV7jsLmjZFHQahtIVZpcKbiMdEq6uGou%2FF29UXIKwND4KrTBlzloxLyeqYaOjdX%2Fg0VLY6FpWsCcKTTuJXRaD1%2Fq4vXlC7l%2BUyJgIycyfRQrJHmrVVbWs7N1A1dSs2m9MFCoBwT6ZWwHsegjVgsC9D5izDGCCJuouAVuq2bE6vOdxo8fR4ujFYoRQgPPD2J%2FUIFOxd1MO3E7cYGOqUB3rykU%2Br8k%2FkXDRGWj1DRxoHGYeRRD%2FohfR%2FArdbwKsrgi%2BA8ToyP3o5H17xmBMZye9cpynvX3vE%2FEMcTWs5fcPOgtZmgtS2dttYA%2BqCZdNG8dswcjDpT3HlizvCEaxUV42NgwdWQCGWB%2Ff0RPOxmcuCz3ZFCtv7cS15ft%2FmiTVnVEy6IngP6bL9IrE37aFbaMv1GhrZG2lvEQB0TNaPSqYch4Ze%2F&X-Amz-Signature=2e74c67d85a42c03c9bcc64494444aa2b9b56fc52144e33daa62dfe474330740&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

