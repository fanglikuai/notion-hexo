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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663IRU6RCC%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T070050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCEYfRW2IE6W5w6drL5Wm7w98%2BOH6qPrNSdEblHU08GEgIgJSKX8IuWVEbZx5ah54p5IShoN04Mu27o3Vk9kEA4XcUq%2FwMIUBAAGgw2Mzc0MjMxODM4MDUiDIXK5Szpns%2FUDwrtxSrcA9TcyZEe5J6zRDa%2BzDvXFrYlxr6Uw9uKs6Tcdvr68mLJdJJT%2FJBKAE%2BO3Unk4Jpf%2Brp5nn91nfsQlU%2FGnVyZPY%2FFgJwxyb3ElyUhFrR3cl51ULuhMkD6SslGpYJH1tx79AfRIdXc%2BiMSYiJPlbpAydqKab0OP9d71DpEciNzwQw%2BFGIHABFv5iKd%2BZbtU4tlbDEADxbU35m%2FTvC6RbHgh5WGyC1g93T8J9ZKyX4bk9ftbPPSxpAddAPGNFHtOmQg0FJyqAiP8a1tVmIrJ9BoIgr6SyVu9Dm113O83Y9rJS54DD3MLBtpzeZui6mgdECi%2Fmcu9QrtsgwkD5VTejc8xtB3hFRSv5UgXHivCDT7AB7FU2G9tMMw6FVwFXupmAqyuSa6rXhOPyrg%2BqixJnQ13GZbA5%2B0z%2FQO%2FtYkn%2B9BLE%2BOHwRqsMz02Dh%2FbbfQ2ZQhHpB%2BaNjpCn0SAZ4vW3ZinV%2BzUuHrbgInei4gpai2e0LZSkRtFWPO5qEHsZ3z7EOyCduJk71Gz33gRFXPPNE2%2BXwshkKETttLFlBcgUGMVdMJykTeEsRPwADqVA8O3BrRKqcabHCd%2BhR54naeOvZ6nKjXR0x8WX25u8n6oofvtvxYIWPPa41lFsJnyWBoMMKEkMkGOqUBf4BBvHqXe3N1HBFO1Gsc%2FyZnd0wHi6UPDFWKEqBLfma9hW0EtP6ybjQPo13vyQk6uTe41gmQtwjQfxYRqdDO1MBt3QYQSAVaeLsJ3iMzaWFV5vfZW2ehKkCtJjJ%2B65Ww4uz%2FKWZO17Fnxy1kPHIjOna9g7MBC8VInlFzLtovF2F69YUMXMjF0uC1JnsTq%2BwNvgC25QMu1coh0caSq2II%2BCOlvPuD&X-Amz-Signature=6579b741eafbd0e41d75fd21a35f7429e08381441f5fef032ccc891cc50caa93&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

