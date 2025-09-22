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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663LCFYTLE%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T230036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAaW4B1q1%2FFzcSmoppxq4Pmcg0Xlp%2Fkd1VRGwcrixSNGAiEAuE89FeAZnyoJCc%2BJ0xSDGJL35PHxBym1u4fSeUWla8Aq%2FwMINxAAGgw2Mzc0MjMxODM4MDUiDFCosLVTycVCrqeG2ircA7NOt0yyNu2X1gJtk7aCs6DdxOma9RxpE05YAJym5uHBL51rGp%2BbDz%2BXO94D8xOIikqHnrV76Tngt%2BMOp7z0sPTclA%2BL%2FF2O1XmkZ7MxHzuAXOEtxSVOHb3eYjcrIWFkeEHg%2Fd9Tm3V4uiOhM%2BInCETDrsG%2B%2FTpParqP7avNP5OSny5WZYmZaCIbMPE2V86O1Yf1XMB26eY22Qy7d0Ho%2Fba2%2FlKRsoPKf%2FgObqbX6HdHmvTHdO1lbfH6YznbQk3jnD8pr53ld7X7za8pZbnLOS%2FIIGpWHSqLAbZ0b%2BpYXWpYi6WCEVpbJ5WdJSFcWAQLlqxSR0tqEPDsCb3WyLllN3OJMdi1sbt2nxlluG5cRLa5ZPbOEs7Eug%2FZlINOZkI%2F3hpLm78Cy5f3pbxQuRx57%2F68eRoh4iwpT38zrQbzD%2BEO%2B7Q4Aw3wkZEeP%2BY2GzNWbHw%2FxH4zzg2bAYDrCejRXMnSqVgu1zXbBuR6Dy0a%2BhTBxdjOhI3qLccnX7WlV1G2unTrACURXs4eWEC9iQU2LIMfWx9Y5huv1ccUjoBu06wnX39AbnwIDwYHKL2O8X%2F7HtChD%2F60fEq0VbN26tJ6n5ffWD6x14g01%2BIAHZl7IObkcI2Yf2%2BE94iAKriDMKj%2FxsYGOqUBt699iEHh1KvEC7oSGI5iAcIF4J1NZdgih8kkaMyVAd6rBBxzs5Pb086fU6USbBa0uodkCJhGeF%2FHm3NJc65aqCTR%2BdJ0Tu2UbNXP5yyIriRggtrROqo%2F7lRFCktF4qFwWY0B%2FwCQMyer0nS0pSkRFdsdXuRCHgo205r5Xag%2FnEuMcybUV8HINFyMd7oPYI4vjUtCdLSWoS75Vj%2F6j8fZ9PzSeUs%2B&X-Amz-Signature=28521e94e71d260fe917e2ca9e08755d9cfe1f26058465787975c9748546fbac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

