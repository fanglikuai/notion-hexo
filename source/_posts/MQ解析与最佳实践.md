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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZGKJEVGH%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T110037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJIMEYCIQCUqIgPrJvcpSG8SXOn9U34giEPGHD8H%2BplKw8H4ACjkwIhAP1FFa3B4CfLSfWxvytChbtNAi%2Fok%2Bexe%2Fz6hOCStwnzKogECKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwUZUYa1YUo1YDyfykq3AOjHmS3w22cr%2FanbLAWNSqqnRYZNwAfx41zC7wDADayNyU35bKkYdGOcD13PW%2BlAiVNKmwTuh6jdm%2FRcbVoVDi%2FJcb0UDn9gEUviC05zxjB8hd02Zi6wgJBSgE3wITNdRkkV%2B4QJwf%2BZEgGz7i%2ByZhFfBwWdUGtp2NydAUxSa8c4XCKsjK8u54AOEl83k58xTWEAW3oTpYuU4vA47Z89COJ3GP2hG8utRXKS%2BPLRed%2F59DBioGlQNhrmWzLzCN%2BUw6igX3%2F9tJg7pmj%2BYrY2UQCH8%2FfNuQctPQs0rnEv%2ByD3ko6LdnGrcPvpyAppA8S%2FgmEO6Pj1Wra0YLs0vDqnuEpMj5ClcMrT2Rq5bwxF8fKzHpvhrno1Z8NU40FopnMKAuE3CHgJbGIBFFZUD%2BGofeELn9QXZxfpoQsqBh50tK4kiiM5F8dqhRi0nnSS6ADG0xV76lIfYpplQR0aWTzjjm%2FkTX6HfWDPPJgPlNkhYgmwUPzJhbI%2FucLwtnyp1OJCMqCEfz7%2BZMNhmkrxMDR7gRYeXlfV5cQMWbo%2BfrxFr1xh9zf8FBj00jhf%2BqfHzwRUShp8vF1WbUbQS%2BIfp0hxfABeGMevXYLMJ39p%2Bmf2Rpwdl7f68mvDm8M2a%2B05TCW3ZPHBjqkAYc1C7gUsfnO%2FzvCuPbk7UTz5PpS4TTNXgobfaUSOKAVUVj9LppK%2FkVdTAsnJ92JVf4pF6oWlNdNam2ViLfYTSNHoRL2IpEghk6o1Qils3%2B035COa5fL5eCBM3yYJKq08Q96JScSmanmt3iDifSXuJTYi7lF%2FQwaRpcHIz4fJlitjqBKUAXqFkM%2FbGo52hRMWA3%2FvA3PvLF2sIYjOvCCGNQ1Iti7&X-Amz-Signature=7111dbd04829d3e0a961f620f2ef605a21dde5d180c569dada0ef48a8d1e12e0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

