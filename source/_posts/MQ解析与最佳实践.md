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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UNTJERUB%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T180041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDdeo5%2FH8z2hkrneqwSwRxj6j0mBh%2FZ1Il30VF2%2BD%2B9cAiEAhyf8MFgIqHI3DpP1bXERutrsniT8mhxOvEcP2SVvhHkqiAQIq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNuSMbSaH052%2BsgnzCrcA0OBNzz8bcPvZbSNojVMsmPTp9hGIrW42PK6X1B0XZ1%2FjxdSTxsfYT330P%2FZvSEAWxlXOjxsjVBfUib0CH%2BVl%2FMYfgvk642BCjdGz9ZFw4vwzvyQV8zK95pXHdvl3GgCQGwQ2Xhc7U8EzMVbMUOphKHntVWaSSvG%2F3VGwyvaFXq7PurvhGyNJ0OWcWz9auq%2BsMTLoIXtMpXy0NVgq%2Fyu7dX0NgxJjMgKgrgWevhhhVtNk9n%2BH0fn5w7HjW6G9fpfzyl5FrBlFDbe4eFfvfOUnt19Ck26rA%2B0Ea%2F9rRCzzq9dhvaRWncmAOSubZnN%2Fme8K4X%2B1Gf%2FQVKJyNO%2FXZ7AiQ7H1X3QVOc3Ae9e1Ga749jPSYUNM%2BplPBe418A%2FYkReHt%2FWBrPLH5lNHsBcQkGAib8kOyl3YkRJ0Y%2FMSTELcf2%2F5MbtLV0I28tvOOua5AYaKp6QIvppLMwOMaiFg%2FzgGo3Rcar2uK4fPu9X0NH416nS2hnhOndiYQDinBUm4QlyaWJ%2BLgiwvJ6vF4znInrKpNYMM9UhcW20xGh%2BIXN4pfmEIkVTxXVmIcASNQ4Wiy4HCV7Ss5ehBuovlpVJ1%2Fn%2BFF%2FuUpqL7JtxNvxU2DDOySgTio9JM9mDDW%2BduVy6MLnDs8gGOqUBlHaaa40nZWeePFa7ZTxJt47y7vPEHACvXKyOEKOqpZDCKWyqnR9jj3z8Q%2Fqml2454ARgXdoqa1w2BCOzhG2o0qcGqQR8i6DxNsUqeG4FFxxHoXFvoC41lTvauYuwZCFxam2xsz0HKFrq62OLFOBpyQ2rhfvkyfi27L1o3toROcVZdFI%2BNfg8kKpQIl%2BVN2ay%2Ft%2BcUqSlbveivJvCsyJOiX9%2BFrq%2F&X-Amz-Signature=68344968fcafce2650396a1adf8f13182763eccc8a074d43bfe2dee7f26a3df3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

