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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YV7SBHP7%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICmujF%2BGHHm3lOP5SW%2F2wp3Jf9L0F5c80rfAoyX%2BTd%2FuAiEA3rlTBW2U2y5Hm6D3lHEGpADi30iOXp%2BoMB%2B5MuCy5rMqiAQIlP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIydr%2BQtcqfF8LiOJCrcA7WEgkadO8sOI%2FC2OjTqo%2F2p%2BcQeKtgv2z2sNWqew0S1QD80cb2w2Qk0OCLw0w2wQpMZjwta2Ev%2Fhakj9pqOeuhMb%2FkXMBHrJuVMCndXaEaFnrwaouGd3r10YQIrhjIpKRKhdS948tg6i0xqtqM7dLYGYQXPn5obguGcjp053fP%2FEE%2FKrjS5qDwDCV6m3Rn8%2FNm8eU5D92sNHM2r1jAU85xGBUZcx%2B5tJfSaYk0jaSkHP%2Fhfg3elV%2FQV2rWrARw3PH1sJzNIViSjFcgYmQfWm1liRCI9LZ%2B%2BDnSi2GK3zRpwnvYPyk3cMkmLd5eqBT4WnHRzazhYV9rUuwgFzzsSpwUOjssZFlHlQ91zUi%2BOGHeQHwotOg5VOpCewXcMBdUpHI3aMpF%2FspGc76HGDJ7AXjRmQkQXCyNSKukf4d8r7yrJQEDutRTroJPo%2F10QW%2BjdlNgcRlFtRBr7o7JzSFqop0x029W0w1Q21aNu1ql%2FhT2QQqSVZulv872iAAW6yg4p6v%2BSAMnKnjwQoaPedUH4nM2nhLJ4qEtekFlhiWiBY1ainEhy9Vb4OMXDi4LmSH1CBiLVRE1EeErtgHm6UuTkzAdEspgrhJWASBCPVGz2XFECd%2BPj4i2Z1Ra2LiJ%2BMIiekMcGOqUBZbCoqvbSga8YuHO%2B9VhSVGwA%2BC0WZyiZwwnq3LPq%2Fh9DSNvNb4kp6JFVupCp1Pu40cxJPzJzTLqwPb2bfdshJVKkzobFZquh0cwCZ3AQ%2BJW1azucH44i2GH2afBkiTMZGSJC2W9PGooWNok1e0YlHROaSMphE%2BF6T0pdtSijjnk4fircsySvRnUXaJ4d5W3Z6iT%2B7E5Yp6Jebz0gOtOfLty%2BUix2&X-Amz-Signature=87237937c22cf8d4c806f7c16114c8e9a811e112caf6fa5c616632313a2c6ad1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

