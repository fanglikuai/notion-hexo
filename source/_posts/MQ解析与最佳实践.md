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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665VE3IT2N%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T060100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQClLMdATMIKvdmiFtdVOWgSn1UiZVy4pPhopKLMd4KiawIhAJ6m%2F833Fc5EoY6UuCITpgLfeOmjLDTGp8rCQrWnAOc8Kv8DCG0QABoMNjM3NDIzMTgzODA1IgxsufzjMvsByut505Qq3AOT9qTKS%2Fidhe7uT5AAdFfEzRuml7v3ej9%2F6QRigkjIXrP9lFEENSGgNjC3SWYEX4%2F4wkE60c%2FEy551qOw%2BkVKljUiIL3%2BV0GkBWuPR3HicjcmupAJ0Yk3jzwfyEZxcR0L46osQywcie%2BbPlTty5ETieqCGYkeP%2BFoB0Q5bABzb%2BVaGXfTHypSRlCA25npgR4BswyC7tkQZ4Xnf64%2FG7a7SQy153zviJ09TjMpl6a6aFj5AG0zzSFlFEHFNXSN%2BrKY1gBOED2rQhrHpRzqheIYkhTx%2F%2BtNR7o4P29ecVvFtB4lICvQgL0wif2PjEFExKEF80%2F6QiKCWQxBZ5xTKkEYQMCTD9IBoLQ1GQvYz%2Fwe34WSrAtgJJLtfpxOcdFzXMcJfKiBEMmXXbqIoqU%2BirTwU4gbyfoujCnAvcZ%2F6Xix2tygSmXyYKJPfcEhTsAkNNU6zw7kuCN31%2B7Mxd5FNx5kI58kFqwxtCLc09iIy8hQSNp8fkw1x3fyf9Pa6IAJacUVgWGirOfzK6tsH6Lv8bSD%2BaU8HO%2FHbSHTlKTYbAHCNVSt2M%2BujZ7UQcf8XGYy7W4NBQvcHydTrppYLBq0wPbzDZFlJXDSjpda8WNCPnTeeKEE4qKdGLs36GgNC2DDahdPGBjqkARSuRhjajxnVSUDzBdmqLXKSOpdXQo9X6gBkgxSxywEuULnizLS8Wy5%2FsX4FmwyBKuLQP3Sw9v%2FKtLzyhB%2BWsAfoRNbfzlahgtaCWzfwEJ4eg9GOtkUmu5MJntrVSRaVsbKLPByIkRgkXk2tVg2AvrHSpwUrR1QvArwWflxOOWfspOLTRuRej0BAVWsBnXS%2FWTMedunwpnRa9EJj76M47DWSNrmz&X-Amz-Signature=5049bf549f13de1f16df0eeca21a931a0c3ee2b22055179b15ba0ae2d3b05346&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

