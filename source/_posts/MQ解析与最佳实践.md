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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SFX2NTGL%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T130039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD7YXZXcJj1Wg0ybLIYNHMdl8Qu5BDPtFVXDJoU1E4W%2BQIgF%2FoVgRQ0JKDzgRht%2BOvN%2FXNjiTnnr5oN%2Fweb52aNvooq%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDJG%2FDVOcxedT89CEMCrcAzCUYnYyhgngWxmiT3kqLWdOi5eerMix%2BOwjLZtisNXXul43ulrLCheVZ%2BobQsRkp5PNx5EwqUmUOdbCX9yooaoUwJzRqKtU44HHJ53Sb8Cu9ZGWIMhyWqeUIOdejndA1WAD6qy4vgk3VSwbNVVq720NVVtvigGNBPXT83HT4IaM3Sc2Fltrlmlx9x5cJq1woFGyNhIEfRss6kPltvTXFmeH97ucpQnFvCT4gteYPfC%2FBjqVdg0f%2BPHCiKFd31BBu%2BZhvecC8vo2%2BE7%2FFXGMRhQN3Z4dWNUFgbxlcm3O%2BAa1C%2B5SH10Cu5GGRjCDXFGPNrTXsUTHkd3wkPvbQ3I2v32kZSdvTeqX8TmXewhduE3vJUamrZP7g7lEJ5B3dvI69cjIIfI47FZT06TMNhescEnleWYn07Bih3%2FSEqSONJ8CeFknAddnFowSoWEnRMdkZtNpkZIZxR1LkG5Ylv3hA268WyQiDD86HSVGDkZk3xP2piEVuRuELog39GqIXeye5OUxQd1pZYKRs7KUC849ml8Empdmagql78MkFD3lxu8KcBUCO4flg1yDGIVOD%2F6DUkltuOvC6PE8X4SGPNLW2sxMAjFCqhtLxZDepAfckvgT5Ypn5C4tiATWcmvJMNXCosgGOqUBIb9iIlg0M%2FL45F%2BaSoJwCjb69j%2Fz6e8B88Na3nnweYMyaPlClxj97OuVWieGZaBmnTslhvbwABiTtoSrzI9I7%2F3X0aic74lzJDBg%2FRlDDACmLJZlTK3xqyhn1TGUk3VCaceSRU7YR84qvKmGGCoBUymWQS8N6puULJHg8lDtnT7VVylFbsRNkqVodL1yj%2BPRPxuC1UeGQDdi%2BFxxd5IIYy43Ymlr&X-Amz-Signature=26315b2cbb9a3db919d8cdfddd4d67d2b0e779cdaedbb0a7c7680520f94f32fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

