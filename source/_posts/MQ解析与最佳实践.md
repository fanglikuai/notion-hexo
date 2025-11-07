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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TBOZHJOB%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T140057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD7MlPTL1VSXykEF5ZsYzsfhkEOtL2j61dFOBKDZZ6DMwIgEY3yqOq9yhuHlKigQU9phP39GI1Ky2N9eunVoFXhYXYqiAQIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL956DbQOKCu3P5RuCrcAwT5oU3ik9h7tzfx5kYwsQia0w543a30lDVle46YOqGeSVDULGTP3rdmpqqSE9YoJYWhdlcK0RKgQO%2F7YXvF%2FCCkhJCPMP9Cihg2NsDzcu%2B05ZFYKwZqCraRM2DOWUWzMULz3laToAZpDNNAHgkR8M304yqX9PflveeSn9nffGBpd0mO%2Fr9tPJQ4oGPSW2rlFSZJbRTbduLUMKjjZ%2BsibNn%2BkB2VHbCqB9oMMBGnqgOrHdox%2FboJLfzzIl1GP2iG7UNqjSApiePoJnhMNPUMIMUrNT6J9qqwI06SZhUBq7fvf5EjcDVBNfIqkx%2BEqjzprfaf0FqTBFfasmyLJp23z%2BGamoi8lABpgWGVxCRUPbo1P1u9qKVzyGNMsmTaIAy22FK8cwZDl9SSFxpmjGQtBQPsuJB9cH2NtSwBcuO9AHQ39X1eEDFVxPaN%2BtjbVK9wXYBPr2hMrQooTgvruAUmZzOn8HYbZ7ymC04WLlcIiG%2FVwmQ2sOjmSDv9iaS32L6wjOgoWHPF%2Fd8OWcQqeztE3Q2emjZ1547D2VF%2B8C3PpMlT1MN9wswBgMyCCIU1k3WWTWvtVj23JqDjUDFkkGspwyzgQz9zaA85tyinWJB80CWyQqpr8aq52%2B3E4rSAMJbtt8gGOqUBnJljqTg%2F0JtwbKNgrQdf1XukdFT3Fei5K9NS58%2FBt8CvjES87eCKwPIkSnjqwWAyzNgSoHkSY9zcocyggoQEDhrJ7OLk4zJNmrrGHFspbYyKUfD7MkyRtR2C3tFK8UnKjKmKwsKt4uwgTXu1Ja65Gyckk6ATVbDntg8fnHsj0giWa%2FiAuxGvCLXBiNTdY2Hn0xulFDpmPD9YDkoflP5gxRnlH64Q&X-Amz-Signature=9cd2dc552185c5343f20a35a01985f6b15314e13e6c97d49d02386317f870cee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

