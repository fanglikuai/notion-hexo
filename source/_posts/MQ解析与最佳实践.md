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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U4X4TKSO%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T150043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCoLEpTkBYGL3fezoB1RMw8xa74zjRltNJEo0BN1GFHOwIgTH5vrWy%2FZ1CiV1WzJvAJFJBoCpcuddY7PWlkMr2Qx6sq%2FwMIdBAAGgw2Mzc0MjMxODM4MDUiDFEkEGn5a4XxMoshtircA6bDljqT8vADuBECZy3ertMJSgwyWn7JrtNIJw5defCCksNgZ%2Fzz264QploE%2BOCO4bmcwtEOeiDb1dgd%2FKscSB8dAuud%2B7XQpAdNB07QPsQQpJ0T1RAfTiQbmwjKL3EQ0sO0dtEjGbxCU%2B14QuOPp4zuwMy32v60Qd8B3TyHOPlPynEmaqQHq0oqqcV3ploYVRJ0VAmNI%2ByYkeBdM%2B%2B3Dyad7uBaK7D5XYQBAsGKejWBFthm7vo1K5sqkP0wuzrTTE0HxmKElLogtgxnmmRRPwXYKSPNuY%2BK30h0kozZYSh8w94WagMGoxFZ%2B%2BR60z9A%2B6fMjwXHDgifOISNycqp1o59X4l2HEvCGa6ayDDNaQiPpmg42gBbDK0Vohp5nkPe5tBxsuOSwRGNqx30HX%2BGyvUaJ%2FMdJesLdApWTSgFq7hZxBjd7IKEWsdJGk0ybdC3D2idNfAZxCgwmfTVzXVoHO0YCglazEFvqFZXEFwHXjcrn17hG%2FsSBiP9v8coKkr6EXoL4Ymz5rkGjriZEzt%2FmTxuxNhmt8sjlEPiMXpaOW0heocRMmNoTbiPwgTjuU8rp%2FZlhqi6I4QO1fA2CjxssEq5%2FNXYZFBg7Ro7UWo%2F0%2FrpcdPMlo8l9NHtG4luMP%2BaiccGOqUBRLJORkmsdXCnh5QctqoX2LZbmObmZFsEasbJjLjYmoaE%2Bbd7tBZJaJw4j0syzMhIejT7LOhphY%2BZK1MofsPoCHr6rv3NHaRSWZa47w9VdFkGCVXBb54YlqQ6kiczMMhgMFgDeys55%2BXSDygqosoVsEX0nNMWymdCzoVnp6tKxhtoN6%2FiARFFb03KdKDeoE0j4iUtt5ZRAx2WRzH%2FCf2I%2Bk04Wz3w&X-Amz-Signature=30fc1ea86e7b43f5d495c529b7dc2516759b7b3e25dd024d6ef3766e9f561865&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

