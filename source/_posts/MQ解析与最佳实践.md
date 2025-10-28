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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UYXFGVZR%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T210047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJIMEYCIQD6x2vX0sMFFpt5o66BVB%2BYC%2BoNrJ%2Fj7bRza6LpK1qD2wIhAPcSNoH4ETGhWygXA2lNLcn8p2mjk1v8wsImYidAvWUiKogECMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyjIiNwdrStUvDXdtsq3AOv2Mf%2Fygd%2FASWAwQPdyqnr23UigoaFJc2s3IcyU2eLPF7fhgjXvAL8Nwbz7ncCI7vusGBfpEWzLX2sGh3u7FO0XJeyTqbxcQsknnsTth74D5v%2FdQxlKtwi4lQN7vMvcswZkUoSaV07SSBZNlazvXj5eMUWJ2e4L%2FZ3OPnfUeES6twZqVvzCql5xPrpOxxAzKaZuYkbK72H3r3X6gHftLH6q%2BaNmP00s8%2FEDV7Q2mxjdLkI2WbUJXQjVgHYQpWrjtxw5XqMb%2BNGWHfQg7LVFslHyqhNbeCYYUYdyGWneQ1jQXnbWYgbaIGfT9%2FENU6P93X4nDQhRUGrJ3v7npr4FnfcZMwwo9B2n%2BD9F%2F8wBSySa7x2Y2PyX%2B%2BXwBamY5wGyDooHibnGY8kFYl61PgsqNLPbqGOXH5Spm%2BrwmlQVr4JWf7lgGBukWSZBnt%2Fd9XRjMzICwVwSnT7JouoPbXhba9Wz0dgtTZoB7wweXhzPGETxWmkSC7uyAaX0aiu5lyrb%2FUVSbJxJaymrEbk5%2BKrFp6R%2F%2FUUUN6onVLSPNiyXJNtgxXdqU8%2FSMO9b7iwSNXRI1QlAb6TlLytzIUKrFrzU%2FaIIOKzcE%2F%2Bu8EgO35owSywLSDJBwioz%2B4r2rqwFjChuYTIBjqkAR06bd%2B%2F3N9PVbe0vNk9QzrZTkIB8e8oLpKfvN7xsrb6wAFRHjnv5UdTcyF%2BkHd3NysE5NS1jlhwqCUvC9AGvS4xLnmDAY2J4qJLOYNJjR2%2FICgS70lXmrjPBrTeJ%2BsNnK6ggsg%2BISEfVF8M1wVTowBlpbjnW2PguK9CF0W%2FP5mfWYlXmyAMCTF%2BYpQX8XIgLSeYKq45wxCKvTIEXRBE6HWMIY4u&X-Amz-Signature=0ae697fe4a1d0b1ceb56b3a3cb978649fb914f4cf388331ad40b6bb13051e56f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

