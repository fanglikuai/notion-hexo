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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665EQXWZS4%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T180041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJGMEQCIHSauRAzyMgEUfURhBVcjzJ8BU4XQWbChBGrrLcfCoH%2FAiAOaJlpbAIae4nSaUErU19veHANapn0AWye4C3w2aYYhSqIBAja%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMInPRLUbS07v2hffdKtwD8G5iRRzX77WX%2FcIE4Bd2oGViOBN7OR%2ByljwvxBlZx8E9823oaHNYY%2BjH8sm1pMuVIp6tRld4yCi8xQ3fFjDLNyxZT7Jp2nNg6NZH2jfUDxnljyDGLVMOSHsZVKTKuuhvLmSGwy7auZzo6KgogtlPhRpSrIhFq9vlfiIjp27RqrlGCBz2AI3G0t4df3EAHIkqhNr8UyP0t52c4OS0UExj4NLLw7rfWKUGWSz6AP7Ja%2B1uRnyFIm8has%2BjfY8leff4%2FspnpnT4vx0vdqs2jVs52ObADNX%2BU1dpV1NeLhZlcCtx7LEzz9FsOwP27mAdPZ7VCbWjZ0pE%2ByV1w8suHgWgOiYD%2BRk1DbsEuLuWqy%2FBZle%2FZpPO%2FFI%2FElDxTqjUbwwpi00kZJLAiIWA0B9KpIuzhuTnjlZ6oDEz6NFgY5rAciD9cxbSaGbrsoYD0j5NYYutGG8WnfKeKfbEYz28%2FhXDvf1FlYytE5iPAPYqfuDVPhfihPDNz8InUbWd%2FY721T6UqJV02lnnMsmy%2FWd5XX1A8zEtMnN%2FzkA64zi9otpqvkuZroz62TIRp1xB%2Bp0rofABPYuMRrJACn2TmEzGrbfMbA%2BqHeW%2B7NImWw6TnkrSsTxseUHVXDvEdhFP6HIw0rjUxwY6pgHzgVfQ3rp2tK0JWqQBZuf7vGk2HK9T%2B4GzFvcfA8ND2X3UDwa4b%2Fx96hyLNTtQPRW0sOarEvElOtisr1m%2B%2BRmiJN0PoZiQxpH8xVDX529YrEcAXpvDivHS98G4qezDGt%2FbIBP5krKpSM%2F%2F82bDgsJAXgvUKL3SlJcX%2B4PufgFGTXdvXADmhG33D6iVNQiBbL96%2BUKwPgFZLnBvvKKDbsZ7Cqih5vyW&X-Amz-Signature=9fb551d8231010a14663413038deffd553a94ee3e8ff2e5d1cdbae2f72a36ab9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

