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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZHIKXC6K%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T160053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIQDHesIzcOt9Bxan09VcinH2Wna6w8%2Bkt4QHjpmKmo%2FFvAIgK5bdi3xrk9jsXLCUnQp5Vf4sB7Z7QsRDHm2WJaZk%2FAQq%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDK4evqEvi1mzF9XvkCrcA%2BkrWG1XvTTBMdtymcaWPVnjCHgxqPtrH1nDuxzfojZnPbYO240eTTweHv8jereJNTYP6y6K668aiQkQ7fe6pj1lXWbfGs2GimnzuaEc5Ij2QNvzbhbjiXkE8d2gIhTJ6KC2N5mYGznRGkkB935skhsHgHBuf8SwRfWNr%2F%2F3gMzMYD96ozDViigpytsJkHNjINVc9PousgC69Uj7mkyCrP9EJNSypT8yTLVuozrxDfrpa8%2FgPSnKtiga2oMJX9oT03r7E4H8nLlhCon1yTCw%2FRmB9cUVmdGgbluR5fi17ChdlY%2FIpeULJ%2FemHQewSTPW8uWV45g6T6%2BwH2aJV%2FXZDP5DPxsxISLXvzyxDog74SjE7ePOexj0CULUoXmM6AE6nGGlTD7yUamawVPTZVv9oLdOs1cfvGtbyUCLyQvSl840FSO9oHf3kv9acjyz2nsnrh9BCxM6hzHaqobvdPHuBhHbgxnxGyP7uYli1pISSm9OaCE4X0iKywUXmYOGID01cJzspLcWPdvQh6LID4bIM%2F76ilEg5nmZX3AslJ9MrnegeqWvvonZ6k5E7JivuNg9WaB3x6mEBfijif%2B9MdTqqXnKyU8XPVr05P1Bd8gHFrmbRHCd9wK4sFs01HIwMIOMyMgGOqUBJ8bhI3jFdbLYHHe7p5sSSyP894Se7lBNJA4adBnx7qEre1YwGM%2Fq0rkoOPcVamtxCbttIf9FutvJKgyUlCj2PA46nMCz1e9awq2nHQsdh0zibHzhR4kTtk7u6Mu7UGH9Z1VOGpKqk4BEkDHWRTX%2Bbu7eH8f2vnIAcDQgBGJmS%2BFvWQpqU5q0uEpAe0IOKsL1WhfniukFUe5nEpB2BZpmYMYCtMF2&X-Amz-Signature=2114359064de20e43d21a90d11fbebfa82314edfc9befe1b88f5f91c3d30bf1d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

