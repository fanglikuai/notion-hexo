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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T4YTHDUX%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T010041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD8nGYQQnzMNYYZUY3QoU4yd6vtZvw0vvPdc9XDn3ORUwIhAP2uDId%2Fu5OsIEszWVwE1kpWZx3S6doYLHCHcjvfgS3%2BKogECIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyphl9lkA1s0q%2FX0kwq3APoIBb%2BqnIvxI8Ohb1mRpOO6pC5SLs7limcQirt4SxJr2DE1kEDDhFt2hx4tcd7nFPQHlsjHnM2ahT6ChSlNoCzLUcq9tQHV6UrT5O%2F67AV0ST7gSWQVM871rEVqnMKywUJwFbBogdvvPWZ1550lR4YdBzhQ%2BlIuED3WHEWxiQL75AhBWrYPoymSeqFCeS1O8ch1j8ZE4vZ07VrHEBpEeUwM%2B0Vw%2FHOF1kS6mrFvc56OR5p8tmMRqCWP5HjrECqhoaBW2xFHgL6wnshT5WBUrhjfRij8vDdF7J4oKLMMcl9k%2Brun3pD7b8yQrGpURzwxCll3d5hWFOqQqwOc0z%2B6mhsZ1j%2BkXLMPVcIj%2BAJyXIjIztiYSlq1Uk%2BDJcIMQ%2FdY5y5ae9EduTr4JQ%2BodwHJwpK43sL5HwYbrhgW%2BGBivy28aJrv%2BVUDgv%2Bna4jadGfVSxWW7jZZrAzbMRshE%2BCa8sSPHq5hoPq7vzEKFxbUyAkQwkKFiI2g8irubNyMlFq6Hy%2BqBYWUyhYS71AsLbkb9hwnH1fwC2g1mXX85znvtDpSMy5V9mKgMULshZEK1RP4TgmYyWq5aOcMQhtWf8YQz8pFOG1Jqbzp0Xxx5MePaYVovtfyO4vmkA8%2BWbymDDuxOPIBjqkAZdOJKbRhWyzHQWGRQPHoboowzYG4QbLQn0Bhypj8N%2FXMQ4KAEQkUZjvqh%2FtCRMT0gW5fF58RIzlhP88oiGrAz8gpJr7IQF1HWrLCuqeBrSb47CW%2F742nq2KT%2FUDr8Thjh7Hf8PbHau28rP8lwcuzNFWCgZBeF5KVGUZaJh%2FER3yuFZ1cyVMikfh%2FyRr%2ByC4yapA6%2F6MW6tiBNrdAeYqitlxIqbD&X-Amz-Signature=334a333da5281c11d446a223e9aeac7216f7f67f32dfb1d1de2f9c0a37d33fe4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

