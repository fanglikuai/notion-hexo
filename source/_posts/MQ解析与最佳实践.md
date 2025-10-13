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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46637UPZEIP%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T080046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDWLP6O3KOc%2FNlN0zLtZsaOUEODTyN6lnmU8m43n%2BWRfwIhAJ2kSq%2B1r87FD5bW6cGPik7r719iiN2KHhuaJkbtUhbWKv8DCEEQABoMNjM3NDIzMTgzODA1IgypUP9f6tnTBnvuFHQq3ANFMtck5jrTqnqHeprDwDUeTMReJJv2PQFxSdgKLHuZ7i5I8fwzBLzBp3viIpcOxCQ6GzY3KJ%2FyLAbjqmkHeAVhcQtor0Thk60shYvV7Q7OJkgF%2Bo465kz5IALzvqFwXvm8nKU9kBwCW%2FWiMbBUPepDq80gHx71nLasmHIq3tRG%2BbI0TfeuyCOb1GjM%2BeTH0NwuFpb%2F7t0zp61RjHrR9R%2F3BPV%2B2YPxfX6S9%2BXSokXinpJDe0nlRHtiBU4L9UFqAiT%2B7BlQbjGlmTke9wQvxDW5TZw1qnSzT9D158BSGOYTrUxmGrwpV99FVhOVp%2B%2F%2BvkTQSNnR4%2FBA2bZhYxzqTUfbuWTMxbR98s48PT6nLLPw8EHfu9DzjUTSuWqLz1htWZUaalRQredArsoWLtVlE8DQJ3%2F28hu3ajWDc4Qyg%2FBzvdhKcz6EhOICcO3bDN5RyOP3UjBuOsNLiqMF%2F6zyfMq5nOCbUqryz5AgRwUHnW0K1PMWqmuhbIgdZ25zLaSMnhzcue8d7N7KmB6XPSrLNXLGV%2Bu7Pgn4EG6x0RtmdBgd6m%2FwJNu6weN2mooCZB%2FowWOiNVIePSVEOy%2BQm5TB64jq2UvcTWwLwSZz8DsGCjiV88wq99VTPsegby2sTTCT4bLHBjqkAdW52geXZmLwwKX27L3mQlT8r%2B4SFmNzgTQzHcNFBbZF%2BECzTiBlwoSh0Kty%2Bny0AcyzeaKeRDMIzHLcMsndgk0tW1UiNp7jBfiQaspVQsWHvFqE6D5zuiYU6n3QnRAL3f0ix25IZmKO7vITLDxPQpaPc%2FignOlCEPM3IocyoKjIY6CK2go3ZdGnpko98RQAYPeDoYLOk43d%2BAATa80AzI0aIfQd&X-Amz-Signature=1f6d56005c61ba035dc71cadb6036074c99b25e9e93c3e2196ca473158e43f4f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

