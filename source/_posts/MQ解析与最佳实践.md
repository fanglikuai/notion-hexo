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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EFDRXO3%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T080046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJIMEYCIQDtMFU1uq3mQRfXLWtVXM6zrGyq%2F4zpiFGv6SHxPKzUTwIhAPZsNHY0YDxmQ4tocSjbeDoVPXWMCIO5Wtpa7HSKiR00KogECLf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyYPiJjLCUqbGbgukwq3ANqwoA3Jgt1mHPQsNVicAiPUrhNWsgnARejOPTmVCphVbHiBDx3pAx%2FpN4aXrzU7yTQBiDa0n5vtVl8PhYYgiz%2B4ykN%2Bbq3iPeWOWBpjQ0jqdT6GhL0%2Fv%2BMigpuCzsxktgYTMqypBeM8YU%2Fxru8YHw0o6cPjLUWtuFTOsI5Srbp9PFg3RXmxDWHsOYz4yZrFbtqBC20ammaUdWleiX3x%2FfrY0vP%2BZxR8h%2FiNd1%2F2nAyiCpGxvXMVNFoSVVhXJOc86nAT%2FUs%2FGOy75ENtQ%2F4fG%2FcgHEcbEKotrus%2FigudpK4p6Tr%2FAarZxBemUnzryfUNMYuFdcyZmC%2BTXocP9GCpxe7BscwB8E4Akbsv2we2%2BS8PU6NlO%2B8UkXoEfxR%2Bn0sQTltM%2FnwWjgZm9wjOCPQP6VxlIgp5g8eMnRJNNRuBOa1yF%2FA4N%2Fld8FJvhhzaLdFUpRYQvArxENv8zGMTjY6XtfJN2MNKMwGOH8EpQwtNXDaMqmy%2FaeBGq5oFO8e5x1Z2PGnnwZNOxO%2BbjG3J4g%2BR6pgxwxxFcQPHev7X%2BlXuV1PMiixh%2B%2BYNAvnQHYYcnhw0aPpx6Bx2XLTyhY4AESxVwJUt7Jpjfzt8TyhzedArbuBccX17pzE0oMdm5T7cjCgua7GBjqkARg6iXyDskH49vFYDHa%2B9Qc9N98NZlqjqwgp%2Fr%2BinQsWQt3QyECPNJV3MsQHFFwakzk4izrd7djBE8spe%2BiX%2FDgCn8fVLQUONWh%2BwRLM8okuFKPDNzhzrzkarFpVGSnJw1l%2BfdYZa2O0ehYSwB%2F9aOs%2BLwzdwUbrdQDT5Ee%2F89xstwrSAt79F6aty2cHAWgQ1ExI%2F3pPtP2zzzK0KNZc4C3SwaLf&X-Amz-Signature=b42dfe7faac9e1b575275d8e1a6890e6a0359bbbed4b75465e32fa084e2e699c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

