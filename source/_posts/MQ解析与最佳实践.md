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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UHB6W5ZW%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T070056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJGMEQCIHLtbS0cVWiUzgUjXMvcVEGVOffvy%2FeUK%2BuuwiNbiT5zAiB4eR6P5UWtXbyKcbNfucGZBq%2FabeUm%2FH1%2BDReEQ2njZSr%2FAwgjEAAaDDYzNzQyMzE4MzgwNSIM1c8k27%2BCOShOITY1KtwDkf8U8XxVkiarqRWyyhdRsYF8OnXqFFQsZ0raKEmRZGI6z2b6bDVoqOf57ntT2nwHiqj72vq%2BxtQeu5rEJLOsQiTTMe%2Bh7zoZR2fNBYkc4ERijgsiBX4nXHny2cEtJFYlb8jRANYQq8nqvIEO6v8Is4fItbmxtmS51lHSic0yfvA1Rkw1pUo52A2AY0ZIwjOf%2F%2Fx9CANCWgeIIFVOgCsSFxVm1KvujHyKG3boacsrZOLAkjuY8g1SzZAqm0boCjO%2BtyLaTUWW7OkxYdHGUJshjnhqzt1Z0IcUgw8RNyGRG0kQpBRxF7PgEddzn5NWBXAiHS65CFJmpF7frELkYOtQ%2BrfKZiSm6GuAFcPsEe5liUp%2F4MZWGlvzzfaZQ%2FbIy88HP8L9TtLgUMkryHWqhY3HkuzU76SQY087qj4136y31dh7r4Xo6q2bS03YvDocC4neZGOqbUSxd8hkblRcpdYoxDDFX%2FC%2FfJdDJVIFsu5%2F%2Bhjye1yCgDajNHFefAhTSC4U6hmw4se%2F93ajhRaDqZHFo8rUTAR8sNj8ErwG9FbKAkSrZmttaK3AyXAsY7RAcHftD6BnIhrV%2BsELkCuZpSrzrXBEIDq5EtLuYbwukqggyvqVbgBd2nOQixKI23ww2engxwY6pgGzJCESSqXZxMnlzA7LLeJaGzouUPIwdNGBn24FBPgVHE5ylz0WlibmKoh%2BwoleGGuK%2FOkaeNeOqtbkNPNjFgmOyqwrYwBCJK%2BpPdoxf0u7KFptDBF90eQCCIgqbUTkDrGpfErvvI2m%2FPi6QUftcRZn7HafM%2FEAWqC1o%2BdiZVbQsSeHvMXln0QS3jGjZI5SrK0iUAYS%2BXc9Xbg%2BWdgEDcNTuudVUOBY&X-Amz-Signature=fbdf8a52474738d50ee00f0ff870237e93230b2f5c49289bc9ab7c16d2cac64d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

