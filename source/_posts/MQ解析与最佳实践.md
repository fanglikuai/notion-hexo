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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XKUPMCSU%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T220042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEz5ie2aGtOPXOlouC6GWYVNFv0pILcWlPNbhqgV7p8GAiEA0OUja%2Fz7xG7fwz%2FM8zUS35V2SYLUJL3jR18MMfzC3Ycq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDEvMLkbItH8dQLlVgyrcA%2Bil6M%2FzA1jV%2FFRpd35GqtAhsY%2Ba3%2FmeUgwQ4AxXNupdJKaAfg3a2a6hLRVrjooHceq2toRJdLpDM9DzwKGdqoYUyQgM2UXibahchM3gb6ERKLqU1vCFFRm7qiHQ6NWodZKbQ%2B2Rn3PjaoQWuxtIgsj0bFgbld1nJgSFysgyPVXd0ZI%2B2NJkTbcvhmotLOC3LfQrCUm20Ru2S%2BP9fEiLumxmmAxO1kqvUGCiBlF8%2FTo0QmEyh1MlVwpj8IV8zGDZ7IapvAHpZKbQmbIUNRIo4yDnh6NC8BoC9JxCSJ%2BIs%2FCJK0L0eLW0bS76TDmDVXXSUnPMvmHTNzP7h6wb3PRFj5lwnHs344GRToORHVM%2BxEpodw%2BU5O5AmiO%2BdkIrF3pPtNt%2B4t9eH2nM%2B%2FKlQStx1gZ18AnVNPaZDXimEI8HNSEqSUedcHqaRizeeuT8cBhX69Zhcz30PWWaOReoAiivRC3vK%2B7ig%2FWAmTgiiFHVPLDkM%2BodCZ3iZfjK7HLFNjyN1B4ln0gHQsHJZ9xxvgOOHSlK30%2FiL0n1D3cwEc0J7GW20MYxpeM0kNLifaNEL%2FqPrzVLi%2BQa4g3%2FxDdabEfKPEpfbJRsUjianLnl%2BwBHtsj81ai4j1vDE38izIV2MMOKsMcGOqUBch9ykB0OEPBwbpQS3h8rzD4a0YFQNGnCWSIFX7strqh8E%2FMq0tkpZTue30ARmnqgwoJhh0Q4uK%2BpFQGLyJsJeTa5%2Fu0VJu6cEaJIZJnj0nyGRcqas2Xo0PoMxDMKLsKeJli%2FpCcCTLghjcAVP4z3Z8dLky3lIxpnRlIDWTLLvXFzeEJfvIIuAQD3a3I02EZllAWpgT%2BDN%2FtpY911GyBFhg0OxLf5&X-Amz-Signature=0917c084f16786cf4f10f84183de2d48dccc1368475749ab5e559c2277de7c6f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

