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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QLCP4XX6%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T082105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICYYZAgpLbF0KnydhJFzoqm7C%2FpzDZZgx3ImYbRk2xCuAiEA3rZcoPCAzbu%2BzTnDM28v%2FuyV6OmMswZ%2BmYfUxVXAIoEq%2FwMIcRAAGgw2Mzc0MjMxODM4MDUiDCxJOJNB15rAcHKaMCrcAxJ5%2FS7cg%2Fu2U5iuHlTwW7U4zrRLFKhlsclI0Qu7bW1E83HM%2FvycNsNSxcXbixvf69NVHe77bucmnyjAIJUX2pv%2BMtr6I%2BR%2BlcG1T5Vt1xsLwZrxuMnuwG%2B8DLDPOQJlZMbV%2FrxWgyPlTMG7JO4eGO16JYusOjLQpjR0mOFJ5BXNIs0CZDaWIViQa7KHHq3tBCtWjr2Z%2B7lhR0IDRK0adK1aJrquz%2BnYT136eOtJbM0T9%2Fq8u4nhPldOjh03F2J%2Fd9NnjAw7A%2BrsAfl2Ne1BhK4BFqQ1hHgyHGMBcz%2BWm%2Fh9NUH9NR%2B648p3uAbsoJFTMU81uAWpl9s98lPf3Rvd3FQup6TevbpZsanBDzOk99WZKOysHvUW%2Frj3dMY%2BByunDi9AW2Pdlrl59ZWyD2%2F8O%2BV1xR8nQSVRr%2B1XSfZB1W%2FBv6tShvsLdZv5dF7ynzcLuTP%2FnkJ7eXK85xFcU7o%2Bf%2BFxAUO2uDtEuuKdK5lDhFoLWdlPOhof7tMKjHAdofWlmLfpYJbmGtqOfny26wLHbVVy4MFgncL36VmYlOf2JChG8DmS9j64%2FS%2BOhpAD%2FlOitQz55ZTx238hMWMjIlogK4MNx%2BNyghYAFHYj6ZHF7jlDZwDmB80yJjy7MQZwMJGTn8YGOqUBIopqxtIn0XQtcWIUImmRb3zqd0nQXqwdTgKTSv0v1IccHK8KN5ZKixgTqGD2N8%2FLK2%2BupJEdNS%2BriScko1YFQVTgHqs%2F0gsuNDpbGBkgFgLtW7giYM448kXv8sZNFJD%2F05XcMaa%2B%2FFRuzxvbtzftlCVTrDE3caOgdmin0nuUntaQNAXQ3Sd4DtmMl6ORWuE0NbStFraMnW1rb%2F5IucVcK72VadEU&X-Amz-Signature=8a8935a15642e441ae84fd10ac0da6d44947217a1ec6a5a46b34a521a7a83a08&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

