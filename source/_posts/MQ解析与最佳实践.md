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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667M2SVHDE%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T110048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDt8%2B%2FclGcVdX1Njs1ND83bdQRNIERLyztTQUk6rdfsRgIgfx61T33tB8dMyooD3EBz1%2BdExD%2ByecQS1npj8upjb7sqiAQIrP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEQuzUrkd%2FhTpyvMvyrcAyl4FS7lDvd7JoBCUKJgFiAvdbId%2BqQmTQnDoRgvoivjCMi9lD8YSzE5WPM5doLah2um2QQqtAZ%2FgdIPrLzgW3WPi16W7SDTnR6d2Sjoydq5H0qJUQd0lfHkXdvQ%2F0U%2F6xm2Xh3Kxs7%2FPbZLSMgL3CTF1rQq5s39SIbSjex86erlJAW7Yw3K1OaBBOHf8zY3u8J3gc14u2%2BcbP%2FI%2BNY2JGg%2FLWdjQLPXAbHT6w1rcDz8NNEjB%2FB155MtmSdjCv%2FBXqS0XA0sdKtMIgTWIWF%2F4dAz%2FxnooU3b05H6kGUqLo55BzqxXZZ69QlOPq9vxpeLOwkefV5z6SccUCDbhELkNcUFFNIrKenm5AvMCLyFdkIhYKhwtb%2FX9LtVbm0qpaXb%2BtwS5o5h26tTTQcusxAUS3A0lEtnWd%2F%2B3zg7YAretlQ77q5u5qiGmdmFrqLYR02u%2FF743Hs5eDwEd8kH9CPElZVsPoOKlPQDxkPLbA74jKpbBUiKQITZ%2B7lwJIPqN9vGuMgxdQEhmNWBAQPl2soO2y67Z%2FTd6Ed7vER0y%2Fabgp5dybpJDZG%2BAweKfEijcRtCBR1RlJy%2F6J5V2vCTyv8NNsTJBsJCfVsMbHmysdkqk5QJdPmFvyn%2Fs0%2FwXio%2BMIv168gGOqUBy%2BrNiB6TA88WIZnsgKFb7swXX81tf7G%2FSlQfxdbZXhpbAanSXJuzDO85X%2Fy0RuL9fyzo3OuY70lCXRGwOZaRgdZBPCIOoq6PbchcsX9aKO2RHGDxCT4PAzMNLRcH8G9A0f%2Fa0vckVBne%2BYXS2wjHPeW58Xx9UwLisRSfDX5B18xniJwD3Lk7r2nCkLJVFKmvbMudIC%2BWuXAk5sscCVUVwauSdckS&X-Amz-Signature=eb978e2d55831a8f0be8b34eb655a740e131e1b85637d75d367da1f3dad8a4d7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

