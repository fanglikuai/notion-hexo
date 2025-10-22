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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/ec123cdf-ca8b-4dc6-984d-4a52f31eb3f4/wallhaven-jx62x5.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QGM43ZL5%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T110057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJHMEUCIGjElHzX%2BGEzbMtusYCehh1GWneE59H3PlN3kzxFuZHqAiEAm0iy8oZqMZfITR16Ue9TENAmhj31ZAD6J55bxSpPVsUq%2FwMIKxAAGgw2Mzc0MjMxODM4MDUiDPNo2X94h%2FUPfton7yrcA91%2BkMYVj1mJ53eckqusNt2gWfRyo0w7A6bxvZOq6hsHri%2FCPJWyQ%2Fnbmbwn8MIxglL3rtv5TiRj1P3ZQk7gbCHyxr10G8nplh5%2FEMEwEdDZl97EWyPemICnv2tQZZlqC446up%2FZxVo8QUe33GgvQQghoisnTxCiLxqHVLqISH74jYWAupwcF8SjP4ZjJvpdVH%2Baz1AuzpIE1tBhixbKZ4rsf%2Fxc6cR5XNQXivhGNFRM7w%2B8peITL225Ku22PlcChypppO8rJJfy05Bsv%2BSOgH%2Fc9RO5Pel5x95fc345Hbhk9t%2BLR%2BVQ1I4aDKBs3zK6LvhBj7KPW5tieKLni3M7JKY93%2Fr5g%2BixL2GhR%2FzTOTUEtvAFeIpLLVHz%2FrCfNYoVg%2BYlDKvPuGmb3bwDr5wn8F6aF2260dechEOnnYzY8IRAKkHFzyuXtpY%2Ba3sljsTdy2KIehtRvZa%2BmkekFH4%2FTjIGLUbpOERDmjOntOMHfbXkNZ1J0PMZtLI62Z0M2sa6hpD%2B2RgdrLSC1J7uNsebnwsEInwQY67CQ0OlP7913vOmnThQE7FaMP3JqnLS3zhsGDqgTfDN492yyHs8OLYBWLsSx4ITlJ6XftXGGb7N0jTenegU69hPwyNFd14vMNDb4scGOqUBq6rehitC%2B55jShRz2hsFJmP0dyJUP9ddvQM%2F6KcGxSp%2FNeV%2FPulh6iu%2FNPMafl%2BZ9eTHq1b6bnEpF1nnPAvBPqBDOvxjJ01x9pqT7829lpdMkdrm1uCmiTqqWuQ1KgTeV7jNhNYEXYZAE2K9G8peW%2B%2BQXTuD96azRWjdbdhfEwRwtv8%2FJsOy035iWicIGHeS%2BBlBh0gACzDlAKBGWyUBNUQqNn0g&X-Amz-Signature=0d0b6eda2bdf98cfb95891ff715e91824b846960d92d15d251aba3b1cb353d96&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

