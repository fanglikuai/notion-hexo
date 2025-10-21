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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466446KG3C2%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T110045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFkaCXVzLXdlc3QtMiJHMEUCIQCmMOzcRmxlJNkSjv9OgB2lTq82iochha0M7K5ZliNCeQIgWqMwkbXgj5dm7kicKb46ZymEI2CRaL5LkvMWj5x1rFwq%2FwMIEhAAGgw2Mzc0MjMxODM4MDUiDOI4h23pg9%2B1YzITHircAz6vxD1fnBFr25N7Z6hbWBp7%2Fa8sxaTv8ZWw1KKiU3HrllhaaBsd5adlbXaw9vzKXyl%2B9MoxktHzVgLqNqxYm2DTalyBqTnwlpb%2BY1p79SEdnSJYDWcPapSmo4VSzrpGxirISP6xwGggzKCnuV74inLkaWihkhgCGoL1Af5b5KQN16%2BUUAntVt5Dg9WVTRiduLMUB%2FWVxv6eqrvqB29EfsEbajXyzCHyUYO%2BHyCQtGXJ1c1QnzIl%2BM5M35%2FAyISQ3YiXogwX6RtxeaNW1bgg7JycHqcu1vqxkUXfPUQJ7m9hNYHkrG2pTFsFJ002EZKdWsS0vJ1AITDEA9gBrufsMmLh1Rm6v02qoBFdlvFG5%2Fxu7z4C%2BdHt7FowQq%2F0TDwa8KnAFDQwy6sFPhay4VvqfITgbPU64nFBkQ%2F8RFsjnLccXiAjyqonuUYm%2FlqMow6bMM9pzluxv24xj6JF0zs%2FEZnz4sHEZoouHsNIeUML7%2Bdi67BPXUEZQ7mZ%2BqZ3ntwa%2BY9FDObVEbV%2BPfzIyu507O3Kk8nhErkh9rdb1QErpijU3Z%2FHscm74x%2BdI7%2BV%2F5Y%2FjaXCP1ycP%2BMiyG9ytKD886b2Im2MroFfGW0p%2B%2FcIv4tSBavAHfbL0thWK%2BFnMIKU3ccGOqUBgbge%2Bb1i94IX8u4lcq1bqTkKndYg65vAmCfQGNvbsgVjrIt5BeZTtEOML3Oq4OWZHbV6b78NsSygoCzFwZYYv%2FxTdgWWPWkqA9dS9h66t3mBYFEZwLzFuEy5SFMvCBR4xh%2FliVCAZU%2FJFX5JlADtAvpOce54cMXYESRXzFQJ987b8AEXFyNkACnlnp1CKNfTq0iKUOr%2BWOfJosICyIIpgX62YNGg&X-Amz-Signature=7a3f014da80ab7964645d976dd3aebe266e27937cc794c0148748cedd5be8860&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

