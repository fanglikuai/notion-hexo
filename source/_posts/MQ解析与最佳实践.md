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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664C25V23L%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T140120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDLySIwcxum8w056%2Fq55OMV%2FwbDkxawzAhcG0JA7hIakQIgROPVBeREInQ5UWTSs40PEJR71vpGMpNgw3DNfBabb7cq%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDEW0iUTDlN6ijlhxCyrcAzykKFvNKl2zCRMYRwhxsNFHkBtzR028lhZS%2FZMS7oyczOERuxq5A1IV7Zg%2B%2F7Rpvc688SIYrFQNqA93tRqOj0D%2BS3IMfdGE0iOExzTTClGXAS5INUdqiSq9Lj%2BslvL5koFa%2BTbtc7zVVKXS5YAX3gzZVZyKMBY2NoZd7Dm9YKFv%2BYaVqmJzDE5k3BcOs2buDKpyhjqEZcrmcQoWGUNP0HrIqafOERUU33pxilDWNwZwyDxx8oKn3WMRYp0l1dCyID8wVvxyl24v7m9kS7%2FCD7Yd8b44QgXM5eyBo5a0mYbe7GdVC2UbdMgDa6l7Qtz2pi72ngVoc%2B0XljOz9fN54TjpR90mJZgahBgBYAbLH%2BeqK2nM3HIi4xiLx%2FaqFTrWz6Dn3J3CfJdyVyVyOVsCOXtzXso9xt5J0orvlkq5j5K8vXJvSV4j%2BvkA32h%2BLmvVGUihyu9kPzbPogSFaMSHwcxNwW59%2ByVxekpzMx31SzQNJcRqQSCr8SlZkQ2gfugnCIqkEazYz1ea68nJw3%2FLYiI%2B2GD6kewyxeqgS6nCqRhOj6v%2BzJ1l2B59c1JNlNZ5JHxMzE9RocPCNouczS%2BguVfei9qDMYJ%2BlbZLCzdLRtdnTQHHmYL%2Fi7YkYviPMJvSz8YGOqUBRgWmLzNbxcaWh%2Fu1obMhez6hVZrmxIc%2Bd0EFyZbuAr0umTCCDjBaqwTwfd%2ByEvuX8tACNRLJN1E60PH6uOngSfDCH3C2biIdyyWGtGwnZUPPPlUX37XdUZNqYXvPFSKTAJdLEwaW6wOd0%2FzPzSw674SOe%2FNTM3bYkLMAyYBm1c46EaiZksHTPVZQKJ4OcEVu1B9TewfjW1Yc4Tr7LFn30RFXL5ot&X-Amz-Signature=ec26bd1db0f43568f1c9bd299a6fd298f7d524c7cdee1813c106ec0130051e4c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

