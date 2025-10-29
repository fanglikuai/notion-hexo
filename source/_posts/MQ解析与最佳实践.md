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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665W7G5HKS%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T070046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBcaCXVzLXdlc3QtMiJGMEQCIDMDqiTYHW6v7%2FaZlcjNgFGoI%2FgCXRpfTSocSbLb1HfxAiAT2ufECkkrwZKxCYEIisVjvdtQZ5Ea6tlkNy%2F%2FCatvACqIBAjP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMeAaaNnyqPzLk4YklKtwDEEZbzSDqme%2FJh40%2F31brK3m7AiTVOVaGJpgovKbzFmluqD%2F744nStMgrQ7epd%2BHKsPdlgazdfGUJlkVhb%2BLGO49jnDXWAhbjx3eCR4trrWvBZ%2FUswfYtHtgQKFxrcAB6zO3vrzhaI5KMgkPniGKNtVRlEVa3IYc7BRgE4yd73Is6ejF25NLQZHmiXmCur%2FRHR9af%2Fc8ZS6ZSSMV6dGeTQ0%2Fz2U12lJxKDSAeDyFcX59CGY5GTT6PGS9MkmYKyiNHq%2BI2a1ZoVSQW4E2DBKYWreIG04vYptqcHBB473KJsM%2BCiNxcNSNdL4X3ybTAULVRAjvHfrwqKiscV2wU40Sxf6lVZgInWxpmtqKvwbRGEaKNcQT1K4Oru4H1N0lCobvUPyqDGaEx6lIoGGhE64d9XpS%2FqnB6O5FRivnyGdvmmrDjPZ9R9UeDD1IMeZ3J%2BXSYOA82GLptcS%2FqO9mzEngiSTgYgKze8D0SZ9sYpUIUFEqvjTxFlSa4RNafpGuCLvgDawO6NnIblxKpJexxcx3vLKSWej%2B9RbeFyTKXMfuDAbSYUIbV3PrMzkXhkXrnyHSSYdwPaCLg9WYwIV7zlCjxZnk5a3oW2gpluUFHyaTWvWYJSVo9umiSprRVrQUw1%2BWGyAY6pgEMua1lgdjTHTQ7b1HgcmRiLO5OyiC%2BLuri888cw8VVk8PBqAinNr3Pq%2BCievtNo0nWN8danVRAUXxjyIaa0Eddq%2BQTLj7L8L7oSCRtdIaKCVsRVqaSgQ%2FIHTKFj5TJHlb%2B5y8dh%2FRnAtyv5Yx%2FtANu1YpohobSYQcmLxHLocG%2BfyKtpjDWoUgi6bcZmbD%2B5zrxOh8PMkdooXOxpZ6q93Z5TSQWNO4%2B&X-Amz-Signature=5d13590ef04eda443d9f4e57117a53c9d370ea831b30cfae4a6c767e3869fdde&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

