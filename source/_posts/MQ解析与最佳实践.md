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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSBYPRZN%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJIMEYCIQCUEeCvJPxhY4bNVtgZM03FMI8LyOR4h2gvxF6Kx102kgIhAJjutMYMK1mSlmbiTvZBb6U4eOC0Ecs%2F8xSosoikcTwRKogECOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxU5yu5wwM7fUcEKhwq3ANseY0XsJim6pi5IK%2FNijVutH1s0UhoS7aMc%2BfLt58iAp9ErMEp4NqpTs5ecUYSsoghCCvxw2QvA8iUeRDkvXSSQ3RBcgxsshMatNI2OYbN4q4z8%2BTOBQaYY7yfyaaUxKnHrEL7tVKdLLIjH1d%2BdMHHLPSWfRr1HYoRkH7aJjI1au8Ak7nPAIKBzbf0t0vtbOwVsIdyVrkNqqisoiIvKXPm0QAe%2B1WCC1DxIsbmS2jT22p%2F5gOAkJQr4AFQZ8P2CVcNmw8UCyKzkvZwie2SJMwT8xbM0wmmtZraoUjqlyn0XILX4nOL0lehV8oFh10xBga2JS0qTS7FuljqszH1BdmV9SFbAVET42tDG5Z2rfVjWW8EZVlYOf9ESovJKdyiR8oKvdzUGMfBY%2F%2BJuXZdiuXETemXBloQsGmNszOkeuCxm7zwj3VHtulA97lgd%2B9fZpEASvw68cUsQW5T5DIh05HwKLI4Ln3n4hnYTtXqjzZZKLAd8CFy7GcchT1HalS4aNZ0BULG2dstgBv5T7XHG2sG45tj7Ai1UAIlHFb3SGTydscaziZ%2FdvFt3oVrU%2FCfS0g8F1tQGtCS%2FlsUtK3x5MgE6SBDoBQBRkbrJXuvIhQjf6hzkzFy3dNISgyj2DCXgYvIBjqkAaBB8AziR52yeYvXjKXd3yp8ECbcPMHkokZdqHztuLabPFHgn0E6n4HtckgRh0e1YihvnTmI8rCObovs3KWTxpGG%2Bg%2FYLZMbD5xcrjVyRYZi2Ww1gQXotu1jacykhwwtdvwVMImdOlIiEgo80JNj3R9ufQCEdsD73hR5YIIkY%2FXY3ojuyD8uAgNmPqDLEhW1VpAPX6duS9I1b8T3u%2Bfn6E6IPYlM&X-Amz-Signature=6ca9d72284c6a005403f2d29bf14377264fe70d1387316c9b44b6400171844d5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

