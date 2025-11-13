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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/ec123cdf-ca8b-4dc6-984d-4a52f31eb3f4/wallhaven-jx62x5.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666F7EGK7D%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC%2BVSgG9QFMfxSBrAnsY6IB%2BSBIIsB5QcX62KYgCDvjHAiBkvTb8ESONeccwP25EmpLrArp0gASwohOJrNraWlSUmSr%2FAwhWEAAaDDYzNzQyMzE4MzgwNSIMcfCOrjUGSYrYOkWBKtwDswkWEbLF5zWFt6YmE68g0ESRPefp9pGApGTd2t%2F5sdSpcplGo%2F9oZpAANBjMP1789yyevJu5eE642Q0IJ4wOInTenArnPLwzUq0c3ayoioceWoFBxbZhCUeqnJ6NXJp6DaQGWGamSjfacHor78BC%2B3kZI7myGkPgkt%2BUMdPrMDk0cx1VnkXV2BXbaSmxmcv%2BJSxnG622%2F5MKsYf8f%2BfK%2BUEJQAmdcrG1IM6KdQf0UYmFj51vk9YuCZcuT2zBDES9yA3WCHDpaKN1snCJQKa2wxrtlW%2BScfTI3JtOFhYpg8EKXd4wXHhGnyrMsW5GZvWi%2FMQdwl5h1%2BFmUwPF1J6XkBo7I9TadVj%2BOIACR%2FpiHuTARGez%2Fg0hApvYQnzZI2vIlZvB4R%2FKtEfNQiJcmW8wBoBS6NfjiPXzwWuadcFCyt92CkjUXOCOWPr6DX0FfDurAcfzsW6R8ykPz%2BDKgTJCqnrD5JFhMyC3v2vdOcHdlYoy%2BRvidgA5glyATRTOCuj%2B9cj7Rq%2F87RQtZPYFLk4AfEo8IRALy8fCrL%2FsIlR3QqB9akXmtcLvhSgUGBqdNbvrgMiU5WaN2e1PkGDhauByVqH06Tt2cR4wxsPEpMkC6Exg7cZZI9zY0zjF1kIwgZPZyAY6pgHPP0gsykxpQTRVPrdAOr6ZxE0qTubTCOrs75yy8FQ7S1Gt5godkpDMTY2XTRpgB%2FKzVV6KsxdI%2F6kvH9oJm4aw25Sh0CWwyhkrruYhoIUWrT4qJQQ1fJjCPyteEO8Nd96Kr1yjmOswkzBN%2BHk2S1X0W%2FnFCnKlzqK7eJkrG93rQlRRCYpMopn7mwLV7R3efKkk6hUJEE0m1FbAOaNuqg3AgdYiAU3J&X-Amz-Signature=41a7483789cef53dda12cb76f1d001d05b97eb39c431ea0777751e4917a39977&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

