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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZBN7TV5G%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T230044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAi89HZTGGbnW8NQ3AkjQmqu9mLM969VXtBtQNVOIjLdAiEArkGu6ov6WBSkGwp89lz57IfkVUlAPcavvmz2u2D3iW4q%2FwMINxAAGgw2Mzc0MjMxODM4MDUiDL69kUpvf6yima1bnSrcA%2BjEY9tqlE3JKlaWZhWBeG3%2FO7zSOUJnYYLKhboh3Q2rwMf%2FQN2Hols1Zt2ZGLhTEmXFNoRW84YntUIOsbmYLFC2feD84c%2FgqYDko2zz3iV6db%2FzMNFA1Fq%2BJMJkgM0naTsRDUjMP4VKG0UryBbldxlBf7Ba4uRcgYFMxlWkFUnrPfqo7OwMYT4IZ4raEBWVza%2Fm5u1ZxMIVbpoR%2B80TkHrg1K5NRP2YgMcLZg4lprCrQDPWvfW79LIjaASV66YaQBRHlGtMnp7uUyAJ6q1K5F%2B3Rj7yrY56AaFfsnBGRWbJiswZIk3AKdRWjic5lze7rxnjQNeVkqbbadTlA%2FtffwZn5G68%2BPWKEzKUgZ8wbGW2FeR%2BE3Mthfx2MbsbRFb%2B93QCT2EyrPCkpsoaHjPdIsiMEjkvIX%2Fw5UJesW4Bxb1PBLCweQ2caep7Ql2HdBGlVPl450Kx9CROCvqbq9K2zQgrMZDsjDOZ%2BINNPdejywLoU4Rea7rs1j5EbxrcwDL9DRwlKcS2ZkXTW7CGXxeBNAPp1uQwsUc9KRD02i%2FWurdRG9K6knuluNR9%2B4Jk4OD8SP6g0LBbX4kSxA1q1%2BkWJLGzUV3%2F4Z%2BIL9HKwYcQGKnN9OERCupWOs1nJW27MPva%2B8YGOqUBD7KaDKZXcp%2ByKhqTMLlL%2BRAKRTohcIcvtSYJG172g3agclBHUvo8ZrhU8CNRzkJ7y1pd%2FAH0%2FMUVzdRoIjebkrk3coeir6AjScrRGCr8AyTyaDF2w8seT%2BjGCi%2FAqW8p3hFK6JaC0dukSPXTmxGspSu9OT6mqvlN%2Bmnk8A9zVxk2%2F7v%2FZ5RfLC5NzfnXkJz6ZoibQjv5tMmUBCyWTBSE7Idy8nxT&X-Amz-Signature=e97c4ee58c8d49cdbe2b199dfb1d5ee36f15a630d3423e25153e578e009fc9ca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

