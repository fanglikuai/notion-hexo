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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665KMBBJFT%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T210048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJGMEQCICBEsbWb2TJskPtLl7h7I4VksPF1YGUtO2rcXDWueVbTAiAb0EpofZUxzcHwaxYtkGWbxxUwn9vl2rtddmNpU5EabSqIBAj2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMt5OeOmCzyeeXa1RGKtwD0pYbqOuUFApFKBds7cpt%2FbjerDLs67d3jU1SKCwaSV%2FGXJ7Bbeb6%2BCG1cLGxUtGAlWGEUb02bNaKncrK0MGM1ErLZWyW3RYdQTuvSjom4782AxCNzplnwyXHbwsVeJt95fh418CRNPSmOav4sIrJ01Lu34VnhOcnPfNCd%2Br8qj4eQcadJV2ok0o5LPM5D2xClP%2BC2Fbd84kBqyRlu7nuOQkFV8maDu0wm6S9qA9oUeBasPuNh8xtms%2B%2FfGkn1dnHL%2FNmSqKrvQX5VpwhvqaIfyDZfc%2BSTCD%2BaOKOnIuf8ICuOXolIRfkz7JWHaEcGawDjsZdkd%2FNzxF9y7DcTgS%2BcXxH79%2FP%2Bt7H2zxFf2A9UBdS%2FnK53BWO2W7V0ICqtZ1J5QC6NuaVWFCGCbNWYUByIXSp%2B7vD57fhju%2BRP8uo1IZXDQN7s%2Bo40Wivu2wX6yssxPmi%2BlCWp4NaOYIjubE6KMP52uugwGOJq9tTf5fUxvJfrSEFT0Zzgwb9H2ZR3EGFQWzCXLMkZfl%2FuZI8J7fqOVNmpnU3rm2hUXb3R%2F%2FwU%2FEiUtnx3XlQ1yOa9snn%2BeBEDHlWJoOhPY3oP3Bu3YhCuwEg%2FR02dnCOaiSfvaAga4BtQrwWVz5sLAwKJf0whsDaxwY6pgHmebuWGr2PiGh6gPpE2iib1IZyz7e1mLfVmCcYgFGL4UXZDnixJo70KivS2NLPk6sTEwakJ6NBE8%2FQP5WxgervXdhNQRrWOPyK%2Ftjr8wrGfS3OP43ixpxD6QXoY%2BbwznXFsqgPwuExtyDVjFJIQykgyBG66TOCtv8cZsdPswlSbM7IhYzDqp%2BFdOfJY4Tg6vHFOly01D4kFfZ9KyuRtk8QeN6GGRKx&X-Amz-Signature=6fb88d5d2fd7420236fd8d4fb8f8e4846a68ec929d7d92812c56f9f8f7799604&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

