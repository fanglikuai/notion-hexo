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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666BZAWLBY%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T090050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIF5x3kl9MR0FYFuB%2BcedfeS8ZrbZZng6TFh%2BDnE%2BXtjGAiA7AkPg1CCw4YxVNo1gYHuhPU%2BJDxFdUteIBuNBsX26Tir%2FAwgqEAAaDDYzNzQyMzE4MzgwNSIMmxx3cyrdlePGlOZJKtwDuHI5459Dd%2FAx9ReITF3mtzqMFPwpf%2Bj92wPeHgJbzPzeS4ncDdZQWUikra6ylaJuylXOkGQ4gSuhuHnZosOPJ90i2fzpBfnh5%2Bh3lAwc6uBqH0y0ENFmZLtps1VvHL4OCx1w0QkIsolIhlatbtErt2wi0WqXnQCMuLpgec1iIW5uUOHaghP0T2tzibArE6%2BYGyICyYeg3BtGf6qiSZicBVSsKcYJsrXU%2BgB7fyN5LAQdar5wmRmHb5saNtE%2FZqcJ7VHDGpS2mX4FYmM3%2F8o0w3Gr9RBQ6Q1ODwXukGPZDDFV89tA2M1ZP1gJ8bLFMa%2BxgVJzeLckg0A%2Fcz2FXHEm3DDPvRyEIUrC4D1ZsmPb7ZkOLxFVPhZ73xalYXbJPoeCeVZS8kSWXGUKe0Y4bEF8TU%2FJnJUdwPGw1ZQcFQWDZLRAnhb0AJx4EQwa6XR2AIqyd8oDhtlkQzMkTsiR%2B7R0NlTrBWK6dHysjZiAFI4JlzJ9h6BYTqi6xpGiZcjUyG80OQDfP9vS%2ByrtDIrnE3q0wCahylEwj8izavRhp%2FRVnsaFQJqR5xYhz3rHnOA7TNm%2BIwBfbbhuTtODEuUEeojIh1y6x3X9%2BYBabf0fK1csWBKK5PS4fy2S%2Bmbm8%2BYwqJ7ExgY6pgFDqYDdS28UHv%2BY9%2FShJWjKplHoSxvwAZT%2BfPUuwSRdPErPJPvWNg1rxCjcW5m2glf8carIun6kDGEaYraValpMBBfl3ddbPZMOeYNsDoSxZkq5UzgrQju6mNHVSBpv9umIfV7RR%2B48NS5UBlzd89al6YbQIscWFD5D0wWQnUBqiCuZZXBVKNiaoNVWkRUDM1AXNDU6iVdz8aEqojMLXx7ODZnryScr&X-Amz-Signature=df629b8af7860f4e90ca8bea684de1c792e6d2a285e98cc96cd4d3bcd9d9de03&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

