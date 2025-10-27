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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/ec123cdf-ca8b-4dc6-984d-4a52f31eb3f4/wallhaven-jx62x5.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662AMZ5ZJE%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDAnKsel3d1bdtDCMQ3mYe3U5tusTPLyRLCPzgvmRlMwAIgHMxP7Trr59hfbFErfESfXc1sJ2t2AqxHo7ybOe%2BrSwwqiAQImv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB6aiTvqPYNMczj44yrcA6nHYTdKd3Z4eGrKnzoPdV%2F%2Fp51ja1M0m9rAeTLxlk69Bw9KbLkxu01uSAovSL2AseLFkucvzqZbajkxM7MdkL568GoZvYS7e4WtNMtVi44XcUlumAgvxdkdVn%2BoPaz%2BG%2FTlgP7Gfa%2FMYCVCV1GP6P2DRdXba7zPVTN3nc6uAv7geFXh56JdzPhVCkRNrRVOBbl3hguRfBAtIL7DI0o4kBgVukkW8aYw4jQ1SwjiUtUZEpaV4zyvBq3vGLG9bvCDeilD3becZF%2FuRbkeUEdTKbaq8%2F3kJO0vXdznTj%2FunqE%2FVJTFJgc4KtsuUmhbXD3D94fsT7au72CkvwWhbfBJcD416%2FDlPILCbW9YM2GIaGG%2Bxysfn9siPn71kg24ulBkFTwn6hNQmw4zSj9YwZhemEIWjx%2BkWQsn4FFyydxfiqpTvuUV5%2FE0T6T%2F7v2cA1XwjzIBfTTSz00AvIBJMv2m2YuhN0rbhUz6dF9FHlhD4dZZIs4MMozE%2BesC4QygNrQReb0nC2MGA%2BGjlQpeeb9b8dMrSP4NqXVHbg5Ip%2BjJK8Bg3CzUahJasbP2ct8izDiz%2BaAJnym6hWn14BBuwu9HapH6YY8d3Mb7q6VpffG8ThyDXcQqy0uogERI7m1nMLD3%2BscGOqUB0hEmwbMxaqvQRaWBJCVMFf6LwUwVY6KWAaml1OqAbOuheKEM06Am%2Bfk2voN1nPEVJ6FzJyTOiSn0em28koHdyUWzPxXB9sj%2BQ0%2F%2FFVjwDkRdAgvUgxvi1qr7NHzkkGJzhOxki%2F5EMdTq8O%2FICg%2FJhyoIYRxw0%2FLQuEtnZuPiLL0KMScMhKnJFaAsAlaoOUY6LD5%2FqoMGsRrsIAq6irLRVhPI4z3l&X-Amz-Signature=db1ed0ff8f10a5fc6af4a8eebe6eff222094f6ca9f0b1c6644bd72776a05b8ed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

