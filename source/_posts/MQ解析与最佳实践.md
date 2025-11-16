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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VRKQVSGT%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHD9LzrOvtwcHQbZS%2BostJxLiRkTNpf3mIxA8VySOOU7AiEAgP4ywo7a8VTJ0lOwgEYbr2eLeZcfnb4FKbzmozyUeyIqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDK5TNSUCYhmeEKFkWSrcA%2BpzANLiYp%2B2hMa5SbNHHrn4AaG2PDFdkkDl54E90Cfb5oNaCmwGe4f6HsGsC9kK7KTg9t%2Fif2VA7U8LuSGoqN2xYsVKVZbIOPJ5zERwR3%2F9A01GJpeo1TIO5MxKgHl22WL6G%2FNsiFTdvs5xFBSTEzsKqjITwk2whI%2FkvNIYw3Qr97wqIu28EKF%2B81kOdgTG2BR%2FysVHU8eoGyCq%2BICoqFeNmRGzP3U3BYisVz9QLA1iWKTqZxuFDy5duAIVV8LJciUm5zTBbcSFDQqOcDFcCn2MmQyxexVo%2FOnfdKRE2ldjp8HpR28KARlugmaze37bvOh13Ow0gFtPF%2FOZYBFYC04vRnlNJWUQWPOkm3jZzQdGcMg5LLS00%2BoOanAI1LIsMqkm%2FnWuwj6AD9dJTrhq9HmWXYX0Mb3fSeBMCwHrYtFxOXJpFWAuNBNQtVdORB%2FCMVNK%2FMQdz%2FbYfE9q%2FbUcjZCaxG39%2BZzyaIlHsclex3gvlnJbgk1%2F3hVJoJZFxXWBGosb6c5ZNHC%2BitXwboBvhz1jVbVcWpkWp0dXQPzFYkc9thU%2BnCvVaKxfC2wqHzH21EVUd%2Fc3%2BpEn8fiWOFRa7HVxw5F4gF6UVlFZcaqgFuixqp5dHcZKxAErg52JMMP95cgGOqUBdmSweKo9JGsYjC7dlT9tfMg9F7MUGmRTfPBPAEse21F%2BovEh7zjSUkfmOevKLAD9zeJiYKLFbgCgL9HGGwEmartGe3IomeYq9EIQLH4tcIGuScKzOwQyOQMJ%2FJmzLFKKXjjT07sddjorIgIlE9rMsmmESkuBcvHafAwHkCpNcGl%2F6EyEMsDOAzvo3HUrWylx%2BSDXpOR2%2BpfaYga%2FHufQZv8pdNY8&X-Amz-Signature=baa5d072a4aea896b086843f65af7a2e649153990b9dcd39c923ef289bfe75ab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

