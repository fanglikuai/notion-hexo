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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZKMPRNBH%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T140050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE4aCXVzLXdlc3QtMiJHMEUCIQDgHmn3DFp2lIpGzxYKKKkU3M6xY1e1aawM%2FM347aOIGwIgT22jhMCYcaQgv7yJ17cifgkS0Rq3v4sPZl60kdu0LWMq%2FwMIFxAAGgw2Mzc0MjMxODM4MDUiDOwNc11D9bvs%2Ffpc4CrcA%2FlnYWloswKgZD6OFYMzDlNOIjPF%2FKai37AnDTSern1l6pgTgpvFNZrPs3yREqOmS3yuQrSjY4Gyt9PDqJ77jpyLkoSkmM4agOR7QzklmnweitE2i2haX9lEGE2wqQxxXzzAz0oTzrJ%2F0xn82HPStw2X%2BdzAXgqRXe2WV1PejEZB6H0vsc%2FJlht%2Bsqrmz1WQZ7VzvdQpR5oCjDV%2Fx4RPvfhNHYjyiExfbSGYhtAF4P5wi%2Bb0hrgEvRLz10LD9mJ2%2FY9YaUpJ6x4Bsr3arLkrr00NV0gPhRGd49qr2MOFJct3eNEOUsuPjz4rZRpBXM%2F70Yxha7qvKTAa6OdItLu0F2dPzyPNFpzmMOQkgA8Br45K9Ri9DdHUzPwjTLfaOfhhfN1DrsZnEWJHiemmMwB5NKfFF%2BVNG3J%2FyLmpcLGaSm2XXmApkC%2FkDWhTzXrpONqKe10GqDZOq9sr%2BmcVskKaEEvZZwlxqN0E5eYMNpO5bJuRM4bYA3uzIVgfkxHfPGW4f20m56dqftcA3k76rDOdHx2Hw%2BNn66HeoTAjVCbmzgRMVGqX%2BXxQtgNSm8F7F3xNFRL7%2BDioTK0NuuVWhTvjDTwWlKbmNR95znC1Zz%2BTjpLgwP8KmdaMeQyIAH6mMPD6ksgGOqUBU%2F2MuUvx%2B6%2F6HRpG6vbh6a0%2FDzS1d5WWpbX4mI5PUQdeC%2FeIRVzs4uWnrZY%2BdjHag1M55Ih3MFM9Y0K6CEEnDAHgjRmpPhxKtS133IpO%2BgMWVtB%2FwIoLt1mNOQkbuO%2B%2BFlwSoNffE2UPhD94X9mn8bmT2lqCIS%2FpdGUhaiNVkm%2BzuoFsz%2FlzdqO1JR3OvcDR0pxPlwv95WkSYhbKGp9Rh0InhFGi&X-Amz-Signature=ac730c45d67d2192772e0f323a60bc963a31b82e0aa828ce5da5fabf25b835b6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

