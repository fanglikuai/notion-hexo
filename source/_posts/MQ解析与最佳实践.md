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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664ANEEPBB%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T170037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDubH6jOgzMPc9w73HTOqSYMpixrzhtem3iaZsvw6SCNAIgX%2BWBLy5jMT83f4EJGXzFFTjIYtbvT0U%2FVCKFNdJgw60q%2FwMIGRAAGgw2Mzc0MjMxODM4MDUiDNWYWYSE45s3HfXXmSrcA4KWy7E66SFA%2B3LUGs5j3HXtPJmlK%2BBopF3u%2BTHnXJ6KQpYOmS9kHN95o299uSXHImpZexqY%2BBY6iACvv54iSXGrAqMT8xYZLABP6S12YGyVBEGeCS00WCP6iWYmq9DOwsyRCTzx2gOI3lGf9za6kLgbKc7Wr%2FLmIdNcOG21ohEarwp7D%2BTsJcoH7wDfxHxErZEdpNOSWmuYvq%2BEY7UsDP5nAD45vV7a9QA3MQPSWrd4RJEH9%2Fegprd0bJuWA6%2BjqiAVpH4U4rfIruePXwXwwowH1wLy1ttX3tm5rCFJHdz99XROjJWD%2BJE4UsVS1jiQzQ%2BBMZcr3GNl4gQ%2B6vfTFVO0N8KBY3v6wLQNc83vD6ZaXeQDQ3BCZcFBqGHHfgOZGKCKqmdKeWHCfUAUJYBkK8ViW%2BUC%2BxAqdXtHdiEQKn4gx9Xo515o6qTQcNHs%2FkGLQAt2JSto8A58RzqUpzJR17ylDsE%2FJBKAaDAMzstKZmLpze9WS8HGm%2Bxj%2FNaKY%2Bp9tjGU3%2BcVB2p7uJsfL6T7SsIaJwDQsvu7UWslU7SFAFoeHxZmT9i5yAyN0B4RLP0oeFDIVETwloyTdGZEtGaA%2B5yyMdWf3AjHdBOXKgTE62iXcYgz06e3zvSwaoGFMKy%2BwMYGOqUB1wTWgT2mxVFRsza4sjd2BIhhjJMjpu8LiD6fS0t8pZJXnJm5vhCRn92jITmL2XEg%2BEZ4h6FaZR6bAu6FwhyBeQsLj0Evrgtq8rlU0cJimVUaKe2w%2BSIuVT4uNtw8IWtvFnzzZEcGIpmuVOoRLzMy8kj0h0jQE4p8Qncb8InboDOLDvezcJp7xF89tYH9qmFsd6s39wJwAJFzAmuEIhLF50%2FRW5pv&X-Amz-Signature=9b5ded04e79004af8c85b4d7c388391a7efa9c1182d64ad823d950bf38bebc7c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

