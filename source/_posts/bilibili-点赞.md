---
categories: 博客阅读
tags:
  - 架构设计
  - bilibili
  - 点赞
sticky: ''
description: ''
permalink: ''
title: bilibili-点赞
date: '2025-09-14 16:13:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/ec123cdf-ca8b-4dc6-984d-4a52f31eb3f4/wallhaven-jx62x5.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YZ5HITMB%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T040046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJGMEQCICVs5pme6x2opQ%2F6HE4OcwOVjzi9HfWgGvWAogWPRtM%2FAiBEYweW%2FbH1BE87x%2BJJRzQwkjbg%2BGk0Fu42vGRYN2SxoCr%2FAwgVEAAaDDYzNzQyMzE4MzgwNSIMlp9Ofo2yxz07YmUvKtwDyPRlAKsQpBeRjTrx%2F%2F9km9n1kvVRzWMPJICYgwx%2BzWZ%2FZ13kCL4BCZs7LhEVn2AARWR%2BLTpN%2Fa%2BHrVTxVA7Ep816JdO%2FxAROxXL14RPWeNQa8R6RgjArAW48iKeJu0arjL216IAF7CNuxHl1Bsky%2B7Mmm%2BhSjNa5rw1bOuEs%2B6xYlchlhaSsl3rBLiovvQpvidySCqPIUKr6QnJrE7tw9WEj7GqzKdz7CKOwisN7dETm8bJMlnEvKK3WX4mbdvlNL%2Fwqd0%2FKmMqlDUlnabT0Nb1IvTc3cplM1%2Bda68IHtjXMxjY0AmcFgoO7fd%2Bx70BaR8ZCST3kRcYqLwA2mNERHXsfodALmGPgLobZBEylpQ7GMK9T3%2F%2FHPrGnHhY9NxxEM5zj4ZnCfL%2BwKLfhDNPaUr2duKKihODZnYu4PQgsZV3RH5VhoL6zGQcGWYQE%2BWDCn%2FW4BZqGAaPeuHd8Fss9vgby%2FO8L%2F1wjQB4p9IHGfRc2yzb1qqlMB129ySYEQdAyh8UfDks5trGNu04wAr8cIxbKRGX4LkPSJDiF7gd84uG7XZciR9IjGb09Ih9nK4btWUZT6BvQQpIGB8pTmHvEP7tll1vWBWGr7rqfuzcHn9vR66YPtfJ66O3pdywwr%2BXKyAY6pgGxraPqJiOo6XOz5Qfkif0P7oM9Pce7jB8piylLP%2BQQ6%2F6mLmeYkWEVgewKS44jLkAcIgwtNQqYLsUoG7WP5bWNU%2BNH8iG45na6OKoZMpVWO0Nv7FQCdvj2oOavfsG3QJCoBqDK1z%2BBWzBujN5exRm5oaocYmaOQvcoPO2LEphO%2FLnc5%2BVb1R7N%2FewyyEbPA0Dr5gdt3HPGP%2BKLt9agLA48BgyxtVeU&X-Amz-Signature=1f287bc251994634fd41ff9d934f5a190f23d44b60b80573aa7254cd0e1f1abf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-14 20:21:00'
index_img: /images/2e052ae27876da4b1d1dffcec9e3d1f2.png
banner_img: /images/2e052ae27876da4b1d1dffcec9e3d1f2.png
---
> 原文：【点个赞吧】 - B站千亿级点赞系统服务架构设计 - 哔哩哔哩 (bilibili.com)

功能要求：

- 用户收到的总点赞数
- 用户最近的点赞列表

系统功能：

- 点赞
- 踩
- 取消点赞
- 取消踩

# 总体要求


平台能力：

1. 接入简单（配置
2. 数据存储上，具备数据隔离的能力（多租户

# 系统设计


## 流量压力


在内存中聚合10s内的点赞数量，再进行写入


针对单个数据的高频率点赞：使用京东hot-key缓存到本地中


## 架构


![images415be98d0c66f1ed67b13a1723120d50.png](/images/4dcb611d834ccbb16132b82f631e7288.png)


### 表设计


**点赞系统中最为重要的就是点赞记录表（likes）和点赞计数表（counts），负责整体数据的持久化保存，以及提供缓存失效时的回源查询能力。**

- 点赞记录表 - likes : 每一次的点赞记录（用户Mid、被点赞的实体ID（messageID）、点赞来源、时间）等信息，并且在Mid、messageID两个维度上建立了满足业务求的联合索引。
- 点赞数表 - counts : 以业务ID（BusinessID）+实体ID(messageID)为主键，聚合了该实体的点赞数、点踩数等信息。并且按照messageID维度建立满足业务查询的索引。

### Redis缓存设计


点赞总数：


![images65aad04c9eced3505cb770d40f1c3579.png](/images/8d7404034032ddf5bbc9330f7b99f605.png)


key-value = count:patten:{business_id}:{message_id} - {likes},{disLikes}


用业务ID和该业务下的实体ID作为缓存的Key,并将点赞数与点踩数拼接起来存储以及更新


点赞列表：

> mid是用户id

![imagescb9265642ddfbe9d9e0446e988509700.png](/images/47d860bd87f10c2101b205b8b271538d.png)


key-value = user:likes:patten:{mid}:{business_id} - member(messageID)-score(likeTimestamp)

- 用mid与业务ID作为key，value则是一个ZSet,member为被点赞的实体ID，score为点赞的时间。当改业务下某用户有新的点赞操作的时候，被点赞的实体则会通过 zadd的方式把最新的点赞记录加入到该ZSet里面来

为了维持用户点赞列表的长度（不至于无限扩张），需要在每一次加入新的点赞记录的时候，按照固定长度裁剪用户的点赞记录缓存。该设计也就代表用户的点赞记录在缓存中是有限制长度的，超过该长度的数据请求需要回源DB查询


## KV数据库

> 不会太多，简略

主要存储三个方面

- 点赞记录
- 用户（最近的）点赞列表
- 稿件的被点赞的列表

## 高可用设计

> 两地机房，同城多活

B站3层

1. Redis缓存
2. 自建KV数据库
3. TIDB

一致性保证：


◎首先写入到每一个存储都是有错误重试机制的，且重要的环节，比如点赞记录等是无限重试的。


◎另外，在拥有重试机制的场景下，极少数的不同存储的数据不一致在点赞的业务场景下是可以被接收的


点赞数、点赞、列表查询、点赞状态查询等接口，在所有兜底、降级能力都已失效的前提下也不会直接返回错误给用户，**而是会以空值或者假特效的方式与用户交互**。后续等服务恢复时，再将故障期间的数据写回存储。


## 异步设计


首先是最重要的用户行为数据（点赞、点踩、取消等）的写入。搭配对数据库的限流组件以及消费速度监控，保证数据的写入**不超过数据库的负荷**的同时也不会出现数据堆积造成的C数据端查询延迟问题。

