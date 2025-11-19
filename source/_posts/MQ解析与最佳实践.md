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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YSHWBTIY%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB0aCXVzLXdlc3QtMiJHMEUCIQC0O62ONbckHLjOAsv%2F2ubXCg0J6szJhYgR03Jao3m2KwIgRRIVAVKp8LJv2G71voVvixxp7Y4L6lvhfaCRtHpv6jsqiAQI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEwDjMSgFwIfdwOz5SrcAxQERd6kmmif6OTdMCLW5kU0coHV8R6NWDB2EMtTk9CzjHHfVUAsE3gcDPvufLmuzitZLKVui%2FKMnAdIVAJg0LzAPaxDX%2FCnJLahA74L4Efxhhjsji4yizfzuPD9lXiAVFLFQjh3onm%2BZAw75tYDyRHwGSFrQ%2B7ZRZCUkYGP87LEyozMZqCLxTnGyLOPgS0P%2FxRxi6Bme%2Fx8yZka7VakrWLRXBH4ESw38GPtS15BUGeoblljRYsAKTuNYeybPqH4gWC2wXso79TOzGMpLFWlOGL%2F2ikXXpsbEACdKzh5ebiXgCfL%2BfFu%2BqgTlHsjpD2TRbTpsbHo5joSkBoR%2FWsuepS0RrFGwFHskD8gVNMYUaRds2nP2Tbr0txlx8nlMj0hvBXOccGzGlaIaB1CcVe3aLVnDGp1Ct0r5s0lThlJMjuJOGgrmhRcJWOSDZw8%2FVtJdWDm5mwnRS3lkmmf7eNAaUH1C73DgdoT5qw4pM%2BpPfiWvkR9Hercl9mKniMueBM0iYSHDaB3E4vb8idjhR8lzsNKUGHZ1I2kHScZvyrIcnyiU3brbkt%2FdBM6bJ%2FHLSEvQbbci8KbSSehTjdnRyNdjE5D1csDERSWzSyPiV0jLUA1YTXsXxjDVHEN9JsvMIva%2BMgGOqUBi4To9y3%2FjV%2B305F7Hv9QZTHEK%2BoXmTjsyqtntqtYn1R8B%2BYzc9kjRNRL6enympGGDL5%2BmDwI9JwX6Rh32PZqDCxvq7CFXxrmd63FzfC4e%2BFVa0khExlb%2B%2BVkExYes%2BqOr1eTC2CsgXHWPloUbHnsm2u377M5O3jdaygYOgfEy2sdgh6scCc3ZB0bCI%2Fem6Ma6z1YAqXxl73KeEqMpyIG%2BZLtlw24&X-Amz-Signature=d0803468bb32c6b1badcf49bbdded62bc8a7564d031e389b5afd5d042205d3be&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

