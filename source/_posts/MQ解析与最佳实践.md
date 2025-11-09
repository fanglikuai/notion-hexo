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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667VRJL2FY%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T050052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQDuw%2Fb4BrWq85t1jP2CX2L%2FKhdOcgM7Bf7BTJeLhLoQBQIhAO4ZXwB9YQuA%2FL9rxfCMgx88MhYPPGDlaNmd7WopNO%2BAKogECOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxSPVGicUItyeWuPT8q3AN0Cowtw8HGxt7yRa%2BAX%2FFWzmSM%2FVWt1%2F9KkWjgq7r3LC5g2CyOxh2qvpevPyDtO8isbGtkm8mrGQEAFQwbooUGSe4nplHTZwD8IE9TokngIV4TrRVbe0Uy6R2i2t3RWez0hUCg3h0bAEUUv6UxqeT9f2KBadOL9RGwyJlEp6dt4IQOLtNdqRTzX8AY27fp%2BOp0vyYzCfGkuHULahXW1uFBd%2FR8KiT0o5o25Mo1tIMQ7QsmXyve5gdPx6GiYxIou1DwzxEUqkSPD7Iayz2OU%2Bt5ub1lX5fFcqMz8GMf23emtHVeZxgOIol3vV1eBfm79pbsd%2FA4%2FzAcKgLo88TCaLHRt10NAahIM1ZVWFXlt8DAw7j2vc8cXrko9BX0MPqavcw3oGixB3uir8Q3oXtdCpXG1rDk1R43DLY6Qlo%2BLotHQNF4OGBKKcEnSLR7C0zldQF0MCW%2BdZEmgs3%2B0I8iVSlhHpUlgleWHHO1r8ul0ZGZO6GstdpZLzGLw%2Ba1UQjg%2F7OxvkMkO4ubmXIMcv0%2BAEnyTxtqGRf%2Bw8AktVwtnbT0xo5oa99%2BsSdwsrT6j56eM1iFcLbRbdyahF0ZIepF9GHFNkmNehspBKhpOD6OHkcxbAfHD238fH9eP6snvjDJ7r%2FIBjqkARkydrzxazCcyVIL3duf5SjP5uxYT8kmP6%2F%2FEUTGXCK2%2BS0ZgFINLqx2%2FcKVMmYC%2FDMObupEOFqky4o6FitjKxInT254B7xOenpjEmDHsE7Db99DQJBhJVw16kTL7K%2FNm88AyWID0zC6yIcVAtw4E22vSjwFuXltFiwoSpHkuX6vkLGDbRZAptPqIPDKw40eBGVTXuI0S5KrIU4ic6l0dtHSZcpc&X-Amz-Signature=c0fa3119cbb53aefa41ffe4e39f6321f0a53f8de397dfe33ac1caf56fb0a9b0a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

