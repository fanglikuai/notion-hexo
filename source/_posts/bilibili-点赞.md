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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/ec123cdf-ca8b-4dc6-984d-4a52f31eb3f4/wallhaven-jx62x5.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VX65JKQO%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T130102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJIMEYCIQDdxRbnQtkoaZcvfcHh9eAkvPoIpMT520z3eiURTa16wwIhANeacX2S%2BHfX5rA9XHy9X9jlojUj%2FurRIjV4MVFzB6g3Kv8DCBYQABoMNjM3NDIzMTgzODA1Igx%2B%2BJI9ZhrvRspRKCQq3AMUenTY5SjHgXwQywvxZqUZwNKo3iBTZZSAOPHBdlrQ4tzJoo8QgpFTSg5%2BdCXV9sp1VlAT0o1UhMlhwhzENWadzilpqKSunb6MYaPaZNWXVxzd%2BsoMKMu%2FtLc%2BPvfRhlNFMJHJgHHF5S%2BZ3OKb1SPiaLAyMSwhQepDdaVh94HrNiUy41pqp4hk%2BjBtfOSI%2BF0024cTMNiezWpJppItVWMpolJoabB2Ckn%2FYyS%2F6to2vDRuJR%2Fr9hEDRa%2FpyY3pfkwkycZWdDLGf5hH8Rs5GSyqXEJAqFZPE4SNzCReZ29LLsezMjD3myFM%2FdGXEvGG9D5JhmWbS5yOLOG47BzGtXuDd38J9u6N4WmtjgWbhOAg4EWF0LDKRvjOesaFYOwhZJXPiIyLZvi2VRB6jWzLCDMpnQlutKT1WSzzVuHs2MycufLyJSFpeH6msWpskm3Xw7g8zF0XbSDipCZgsLXD8vyjX0eX846HhRKxPx1Wi49dOGtP8p4mVHdCyHlP1TMWtt34IlSLJlnoz1eSIv9w9RoS843FyaLLQHhi0cpbKu5ZQadoHdhbiPIM5tqd0PIJ46nNX1YPDiSsvs%2BS0IU7mfBanwCikpeL0tZP0qZG5LVvduCtYGIJGsTXqBqkMTD%2BpKnHBjqkASch8kNwPaRPiTZ1tVxQD8ED120brzs5GkKAGkuapYXJZ700GJaR6YxsrMycqh950kygg428cFn8WP8DtRM6cQKrrl%2F6KeUY4HpFAeigbIrilwua3VsE5E6CK24rzZDVvPnXkZcAW6kmBv9iolZp55KtM3MCA%2FQlVsn1IjUtUKMTON02uAhSISxwulh%2BIxlmd8jcoDuiO3AHAYhQgcvRFjaHpg5T&X-Amz-Signature=17246541fb276bac2caefb21649b296ae926e62f735400a049b348940204a46a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

