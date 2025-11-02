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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YYKBOBOZ%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T000040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJHMEUCIBgGCpqHdeKPKgg1zlkrch%2Fw1v7uGfFW%2FpNdXgZwTavBAiEA9hO7V%2BuCqeIIvu0yxNUCV%2FGaNKnAH5UXSPf2XEwXowcq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDBvJaCiBauDMRSwgzircA5uvgNxMJzHiQwqm2K5PVIPdGGV4%2FQ87FrUm3FBpCJtDoclGHFxPGm2L4LEtiUp9%2Fic9Fp%2F020jL98Arz%2FC1t1tEVEMci%2F2JLSRVUXzKXG3XoS%2BWM7pK7ZDb4%2BrGljxR02W%2Ffq1T5AMf5kjFNLH%2BIFsPQugHOglcNHbkPyYTSKPrCi3GkIuAdurj%2FQf%2Fv%2BcRyQzaoLPSF07V9fxlrQNX5KjPe8F1oPc%2BtFz8JrbYqhhT4uhgR4qdUfX73Vv7FuzLO1XS0laVfEF7gpmnhRdTDR3rNFwfCRkVDB4TjkSdHvNC8xoIsMgfquyIgx%2FZtebjivixdVvEaAQkl2iCR%2FPkhE6nkJE1aQfZGOOEkHdQNuwSH4vkJNC1Vf%2BToPpegm4bU%2Bj4CPYD%2BJFMjYl5M4hPqHpFFerPUT097zcgv%2FIBz682s0G%2FMMvPfpVbNMWCOVROqp9pVyZBTc4%2B%2BSUjhNU00dka0AMWZNeRkJMKWfj0a4od8MSrTbBfdQbVgErYEcoTAJcHG1UWBT4MgSayvV30GJAdadaoPZ%2FzC%2FY%2BR85KnWvS6uRBCEQ%2Br0%2BzeDPCMVOL8%2BHTMrtvWuN4cRPEx557NpKYD1%2FiFW07kNSZZF3cZ8SI6nGSEvc3Z1XE9Ss9MNLCmcgGOqUBRaB%2Bn8d4tYHiz137PWUz%2BkCFf4Fdrg2RBX2q1YJMyYMT2NX0bRasoCedTC%2Ff00f9N4svGu24ee%2BiozFn7mAKY1X5uIjIO5mA2m59TcdaxiiIxdR2MlzrPm29uwB0rinRISpooaePseh1sUU5GVvjp5hBP9Jg2BY3lINL4ETv8J6eviz3f7FQiYUdjM1Urm5DCGSdz0NBcxNOQUrl5erKH9UXkRYF&X-Amz-Signature=dfab873e56400b6ea3dc9075a7e6188143289d76ab7f2ab4a6d52707ccf08bb7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

