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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466STEDQ2QK%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T200052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJHMEUCIQDG%2BteVO%2BpmRqzG0p61YGgEcLKXSF%2FQPtq8GXAU6bykdwIgWMzHWc%2BRWBlnJsh8omOQBbOLRj0SuoCrVEHxr4C%2Bmqsq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDK1IABTxFE5FONMApyrcA4FcRRZ0kdeFD6kbYXH2EnNZo2J7MpYm7ZC%2FBv0KxqPP3L3fr04t8idtJUk7Cn98vaswXzZp1Dhsy1nAOM2iCG1VlejVMwE2d3otRg5%2Bf68GeeUvjoXBnHe6mw6i3w%2BfRc8gBCaxyrjI7TwKJHkLQ9TOTMXxYLSMnsfmAk3mbFeh19UCYNCY6uGHXAk%2FIB9IjJO936PkqAm2HV38Imw6hdTXFOYA2bfUBZ1%2FGLRC77z4xaVcxVls%2BIsZyCARfs4E0GeHuu1XycyggB8ot%2FmNHm9W9DwdziI4q%2BdEu5fvY2Dg53PUsjbfvy%2BvYXSGYeWWpW2R4fsLEk9oloS52r5Akt7Cy%2Bh4BPSeHCB98hvfEJCoqucBSiQAJV4uihoCVvz5tjNhKYanTGIRigq3cUsb6owbjzmVaeh%2FwAdJWaJyJtiMayEE0aty2mDfIcEE3HHY6qWiAQ4okppuExt%2F8VMOU8NRkBshy3efhhM%2FuFbSyWV%2BrAe523TZuhRu4kcZJNXfR%2BPj4wI5RkEtiF62RLq2oCaEX%2Br1RpiJlcNzh6yvLZmuBRmlgG%2Fn%2FqFq7a%2BlOtrYIz5uI%2BW0zgCAhfvZsl7mCqbYQgEJr1Na5uTJ9GbWMFyZJco2jgzYuCn%2F5jW0MI%2Bz38cGOqUB3TKfP4Uu2pFuJiI32i6UEtQEyouTleIO1H5SSYJG9HqlnxjaGGSmaXFKqx9RF%2FcouQLSD9%2BcKDYG2SsestpusfGKPouE%2FBeFtHYyq3Uw3Ht8T5ZQ22sY6v5alPOJ9T1LjEdoxnCIf%2FSLP7h7ASR35W2P3p5nRljqPpkFfhnkdbfdjtc1Ua1VzGwcooIC1CahWvHFjLicLE6dTrJkGmbUvdI49sY2&X-Amz-Signature=a084ba358163667b9b3381b1d49eabc64feb643828c8876ea6c4f68cc2df754e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

