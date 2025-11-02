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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XBJAR3GL%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T070040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIQCYK8b0ddejPZ2YM9f4%2BdCzqL8sZUQy8tZGYzaewGECYgIgSvYojzrhtKvHW%2FMJHjY6LL53z%2FtwS1%2Be5N8nyxt0L2wq%2FwMIPhAAGgw2Mzc0MjMxODM4MDUiDOlcvxg0%2BfcTCS5uoSrcA0ra7OUjr7vTpGra85b%2FPgMYb6V4Zqxx7rFhAxnsR6LrOZb%2BqsNCghgY%2Bqfg9RSdsf4cDs%2FaYSNHNOV%2FPdSEHjBJ7rY68R6xdPbtE%2Bl2BoxiTCijQW9FAD6PWKowPSicVNT2itrbPA3pPDe1e2%2FpPjQVxAE1AXi5ZybB8cWOQc%2FWz%2BMSFRWcKt7zeIbqUWn1Ik4fzbvznZHKx9YTRH6vuxnYUZVsyD%2B6hqGDuG2eRdZbsn%2FX8Wrz%2BjEkuMio8C3m4%2FdQOpsJhFKpJy0T5jVdJ3%2BRMd4CbzvB0RtUSinxSoOc%2BsE6N%2F6jHe7kVEf%2BJHu5eawR2DSpKN4%2B%2BlzU56kp3VUwibk%2F1nTrXOWlMGTsOIXiCjVfxP3Aq2upNvySQNJMNJdiZEZbqwNQhkpY34rB%2Bfxsy1U8XwsY4OXMgWEYhNdsWnAXEpG0vkkNXbpyOZwNfNdzZXCTAusgCmdsW%2FTSwzDfp3Wa6pFK7UrL3kF1lZAN5cUAC137oEi5mDKEcLis8Y0QuhPzNgnp3dsZ9eHbc4Y3FbgqtQFna3jBH7agpU3vR6f%2BHFTsDkN16yXoW5HnWPb9BeGp31zp0V02JKQZYN4QsFbcyupTBCJrdV0mTI6l29QVP%2FrbPtxH%2B%2B9dMLnUm8gGOqUB%2BH19rBrsLC1anjaNKkTOwW78gap7rS9EPcQbZQjpv3k20l%2Bm%2FjdB5OldHKz2274KoktyUGfM4MMFSN%2FZQTX5hlYXTdKhYCfC24%2BAkljp%2BbLd0GkE4PjS6vDWYp8t2KMSo3F4n8f5F9PJDXbL%2F7%2F%2FbJSFE0B%2Bm%2FcY0lVaH80uP%2FjRGNJF%2BJDcyWtEyxGEOdGcPNf7429EtZbBnvQ9yYQ%2BcoBTKM60&X-Amz-Signature=6e486944e8c22c198c6dd2ad814edf3db2f608730a3e585cf2d8355afbd3f75f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

