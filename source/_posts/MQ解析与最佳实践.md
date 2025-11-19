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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TDUQEJ5I%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T160049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBcaCXVzLXdlc3QtMiJGMEQCIBnwb9e6NFMGkruwcDSln3xrEaX5wxazyGtfY8jP0cauAiB9QA58BHmyGft1pQRam1uRCOC8bIVq4gh9kfwdBnVV3SqIBAjg%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMrMa2%2BvqhzGuoN6yDKtwDJCDcgvVGpZPOsk2nwKXmvKYSnuiV7efjUQ1WD0jCzV7PQT2J8WctsnemrwwPTYF9QzSMGw8B6tnYgwKxfN%2BvQotFIj6H5nOBlr%2FYtA%2Fe5SgAN3ZL1szLJK1FFjEGN%2B7hJBRgHVFI8lVdNL6U9x82lixarYQznO%2BpQq4u4wqJNvN9nd%2BEgKNq9eMTwj9sD3KGTRWw404fvXFnuUQ%2Fqw3R2w9bDnxX6BF4eshDCak5AT%2FOxJuonhBEmQ0Q9ofJBoDRoYw5%2BCISYQ0peX0BF%2F4RJ8zTqgonYAw8Dnnd27DD03YL7tYkjitXFcg8hHIxEexZDmbeYox42q%2B7OUqqZ8ALpeoqAzh%2FbXustzmevjTTOSouwKz%2B19AfDHLtdJo9B90EqGrrvjS%2BiHgB%2Bgjqfm7eqvcqST5%2FIfwOqV12gCHovHDyIMRMC59mjSoZBYGSl8wOMBGRcD8zimPrklfjIQ%2FFr0JpHGftirhGLMnHmOm9cgn2Xp5TG4%2BTZ9hXiEvoF6NstpHCMjUOq5hJoMfdBkcQhoCn9%2F3kRsW2v8jpDEbV4vfWv4gWa9NpKSsslrANeaa7As0TO7yQ6xyqLd6Fn2iWBrMJ60C2pdddbScAKP6ayd%2B%2F4gZ8SpIPafy9V84wj733yAY6pgFJlsvy6eKDgxl0r8GySp6GYRKs64ad3fgUhDIxkBkAUssgqWv8b3Zt7M2j5oPieOAGyCdA6O5ZgfnvjDBn8OHWbvitEMVT67mdhqveOoljRtpsozBuyH3u5t4xKLM4FxW4ABOrTJ0GSq%2BYqXaTNGT6YgPWySNpVlmzF20YXakU1s%2BCmCJh405G5mX7onMAdk3yYAaQfprePhanJJTqLBZblKyiCTAS&X-Amz-Signature=5c3ac8462c20922488cd56df2e4cd51938717f03b2089ad77c4dbb70df301f57&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

