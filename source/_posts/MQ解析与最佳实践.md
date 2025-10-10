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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46673722DDZ%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T060043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE4aCXVzLXdlc3QtMiJHMEUCIQCm0hmBLsu7lqmJW8gSeskxi40GCZ5vznh6suIE%2Fcy%2BMAIgVBfV4UgCIgKGSE%2FEUhrNGFoFV11aSg4fL84PzOnkFtIqiAQI5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOjNEFRswgCkrbbDDSrcA27b0goi20pBwCcJSRMMkxZ2PA7NHv34lGPuruXLVNbNdWQ%2BPZ%2FC26kNVG%2FITUebFR9Q52hZE1Ovm2F7txwTIeLaK4F8CxGVRWCXFX6JPXMyd3fXF57EoxsIsnzZA7JEgfKhQYglvTeiLNhHE5Rt64vNOUlmFXyHU%2ByxxOw1YTrWfgkmSSXtMYrQi9jUL8oJ1o6W8B3Mc2dE%2FmRkPORivuOdXHAO3xdiJCcfK1%2FAbJGqPkSFtDxE%2F6Pt8jkD6ExBk8ziePWftx6wS0xgQig6i1X33jwgDFhfbJIRWnii1LzFOTLWOuhdF7dkkMW%2FiWwiTfrHApUlQvt6MeO2cXKgOLOe4O%2FLSNk8T4kvvW%2FupJvkDVCZo7vIsNfFWiCVZodOZLRgJjXU3JE9Gq9tYcdv4RUzWleSzUJXncy%2FqjEiOXVR%2F0zEplB2QjjB2WsS8svsr6nOULS3WUUVJeQZ%2BFpIj8XPBXqOCixodfJUHAvwJX6z5HLwiHaRZqBz3GVwFV9L3OyDkDaL7AZnQ8pDHPCPWqpSNZ35PuTBz6611ombBv8XO1tQDMPA6fCd0St2p6qzcdCwU8C8qtW4ggsbzjBYFLuFNfm3mjdkRA1Gtw3xH4H1MzENPzpHv%2BzCcwpeMMy8oscGOqUB94%2FQzNSwRsno9abHODfgB0HbPfVKaawC3wKPkqz8H6EK3TmZk3hU8fMsvwLI70FjqTt%2BDYVbCLuPL25aMVmjsSkWos4hwVuhtBYhpj7RkZ3MtuhAgEob2PsnrXz4OVjry2KyBqaCoEGOmogVWpI%2FfMXEA%2BjK3mS4jUp70ECX4%2FLQx2WusfOBbtIEXBIWvLUbGK%2FU8ZHoX9Ey4JoS9pKNvUnmY2Q1&X-Amz-Signature=4174345058e904d39ec0b22712b03257e67bc34145e81ce451b05a0ca1af9799&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

