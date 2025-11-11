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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YMHWRFFD%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJHMEUCICJe1lfU%2F2MCYSLLgF0mIwN%2Fn%2FaPKG0EYZwdDqPFTDaJAiEA5Pi4Sg37uCB1e1lRGnwWdtrboafq7pJcTwHANgwb4Zsq%2FwMIJhAAGgw2Mzc0MjMxODM4MDUiDI16lTsP25%2F5ukOe%2ByrcA2gOY2lJLqNQcnD7L2D%2FXMon522ZuXRZ03gOW9asWMOJlqLTTsex9XtU0uzdeZiiTqj9y2Nkltnvht14q2wPe7UYLzqFMqK5DSCHJwo%2FASZw%2Fp%2B4gVlAQ09bOkbmPM%2F7EQnO40Xkw8%2FmqCez6r4izY%2B7NAr%2FkcX3Y2o1w%2FdiVSll7TvX3Fk8nyGy3M4mYQ29wY3N1GhIQIfKWrw%2BRSPOVGdFbnlX2u6kXHV9BeNLVOJn0FHpls8ZpzI1zDTclbSAwiFICsvo%2FCX0G0SCEWY1KNAs05KGjvXNxilDDhENAQXrKuaik8g722%2B4LZLUUrjLZ%2B%2BHjjzaS5TO9uEIvyuJNUapy1Hyx60F%2BM9oCEtoFKOZMMG8SA9vjhmGlqbXPJpdeTjFsdgi5gTbf8jcIeJVG8tAb2AMk7XKH0APON0u7VBMgenNzls33WwvAOG5JjDgcNh5YUavnnd4ZJIcfkNbxggpGLKwKhr5uiIFWvAaG4Gyb%2FaQyaXiQI2hfUEvfB5eY1yHAaAIKhYptsfrs8DJ9vnmg2xpyeW%2FSU2l%2BMJKZEWabT9AbfQ7U9ldpH2FG9pZlt%2BLtKhyhtUUKUzo7PqpGLGQnF%2FqTEWkSegvQfW%2FyPlU31g9DwQP2Igv1jQHMKHIzsgGOqUBGFxhHAKh3We7tSB9YSYhGuF2Ghtll59Z1%2BjNhgtc%2FGcp4j29JQneIxD3Y4B4HQ8jQ4NYnjbuNdYsXXLXXVSI9z%2B2kLgNARMQzVaxNNfWBJikyGbaG%2FESbURYmdkZZU26%2BVBTDr9zSPhpr6EaLZ8lLuQNmdh4TojlS2ff8gm%2B6XA6KVSiT8s1VObmRGV4kWR3uhXK3FXa56pG%2BkwTa5OPr%2F82hEnz&X-Amz-Signature=e50d1265f255d79fedab890c90cb9b84ecdecb7d696cdb6b605ee2b22248fdd9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

