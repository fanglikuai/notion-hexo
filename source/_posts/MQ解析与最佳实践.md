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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XLX2MVEX%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T060048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDyJfcGqJKZGfx9pdORbmDKjps3Crsb36FDwIZRqFgnaAIgOsLGPPknhkKIfQXuSTyMpZvr5LgcXMm4DVT2uqCrxl8q%2FwMIdxAAGgw2Mzc0MjMxODM4MDUiDJQQNw6yxa3EARKAISrcA70XMfmOXoOdeH1D54RxPOmeRwCHD6VE7WojGHcGHfetkwhAOzlvgM3c2SWaYTx9w5VT7jrb4llpHmFyNK5nOGCjAECFdhWvXQBYRWJlbJBxJfoZQOSoMTO8bRB62WbseVKoy3CA5ZeM6NfOlQD0lm%2B%2FV%2B9LL7U7uUufJ%2FOgSKl%2FOc5EgRIQ1WP5Yxj9tIyDATs44gC6c0Bt9V%2Fm1YgLTwZ9fKdFQXPatjoj5cr%2FQmEIGIkcBUBhloGRM%2BOBj6QPwNd3KwlrO0DRLrC79M78QH13K6p9mOfQL9bGOkCj9TlHN9qWTZ8eVDLyiiuSKxoHRqdIauRQA6a7DWMtTYMcddS2dxIJCqJaBK76VulRPwsYtcMpfQqOYMA1baDcKCD4zScs78y7Y1i9X50g5QS0pNmr3%2F2UmcabTEUbuCkHaeF1gBxPkbxYCUknwgYC%2Ftjwzu5fCZ94pWU6XSy7HMwhbBf4wDRt2250g5ZQgfrZQaODhvOtcXg83SSrEsk%2Bbh9YcBuJPDWUj%2BC0%2BXSSrR%2F6aEDEZnrtJdsLNDgmRF6HGsKxGsXmmmF08kZ5qWWCGfyz2Tmu8WcfbbEp2TPBO69jqV2Zaum801VfKi0drpSwAFWVPkGsUKt6f9iFZ1WgMJOi4MgGOqUBAuCEj%2BonPcq72o5e4RfwQYxvgVhT%2BrXghJcD06kqk4qwrloSDTgO1qBvEZxVpvnqyDdYNy2%2BX0n4Yw8sIj2a22rS3A1YUVjk62gMsdrMOwTqy2VOrhAJXhz%2B%2FQiWHuhd991nkEmsaqUZdV3ZloQDVMutfmCBlCG57YV7336Yo0ab%2BlJcEQJqgv3p6GYFPxLolJd0CF3ZBZnbGZkGSoVhGo1csiQY&X-Amz-Signature=4f01c825d8678c276f1135738a0d6602c2647dd75038c774c69df59a8541c9b7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

