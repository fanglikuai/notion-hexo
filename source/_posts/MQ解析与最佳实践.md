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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SKGM2XBM%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T010044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIC5KArnmxZV9k%2BvtS3Fl3I7x6Z6YaRHSGERiqFzE6kK8AiEA9oYPuR7KnRiyjUYd2HrjlkGc%2FVUpqtCGYBTNJomChqUqiAQI%2Bf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEeGLNgTbe6Zqi4CoSrcA6i%2FqaNmPWAuYtfBeKs6%2FT7bXjhEUdFNyLWNNlXa%2Br2crOIcIxuqjZUAOaHn1b15N5l3rmhzF2ZGLNlqwXeaJhZ5Czq5uE2ZI1EOlBRdHqXG08DZrbI8HPuiGoG2x%2B6lvOTc5lGOGqT%2BdXzqyCedOAgjll9PiOQD2TRfCQkNCCJMUhRa7UTKjLBdVm61HSEEJtdBaG68qlGSoZR2pwH84rQ%2FYEvHHsUGCD5k5CvNJBwAwk2VGmhNKUNvIC7rSYbs2vF0cNQ4LDtWwTKHJB%2FhrQTF8ZxAazWCScFCTOU3UrPsDeiIwv%2FmrObYT23DV9YW7%2BUNh%2BxiNiaPuMb9NeqZdzI9SEpA2tQO2aHw1nj3IqiTv4GBiQateKJfXzUDPhqWnImohXRWqbocc5BshpgYvckXeno1VGBGqgW9LKaibdcmSPHa%2BwF31DjJGuh2BT5MVZTXg2d7ygPDrcIW1NJZhqS%2FcIF1XD%2Fyke%2FXh21EgHLnmjVGXt7Q758Q3Nuk3BHpb37e29y3p1JFZYM2lUocqG7XWZtC5PtaEi90Y5yFWBHn3ULhCXu5bABwUE315NH3zCASsYJOjeOObToqiNYoOaO0ch93qen2V5mmP1KyBvQhsNcw8etjKZRwwmNxMPnxj8gGOqUBF22bz3KC%2FhJI6vJxq6EI7bvU1jkm6Yv1q6%2FAj1JXbq4sHMsFUQQ0GaPMoJdrZdtNQLHzdn34WgQU2p7hZ%2FxfUySGYSJZXZzXNIsp5gfexzLHaiU7kzU%2F%2Fr8veOazJu2ZWGEQbI4EzxghEYojipSlYfh7Tk%2B6qIupl9kzYAJK8asrQCxiuWx4QaVHLclxVI1fte7owi0RngZkilf3UqAtKfgjiCPw&X-Amz-Signature=a34d6cdc54abd737c046571c753c5313debcb154bde3f4f42042607797209d13&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

