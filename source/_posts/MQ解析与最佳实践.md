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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667NQ7MCK7%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T110036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJHMEUCIQCUUBJozGnm1fd2bdikHWDEWGT5Q%2B59eRNjLmtvZgJcMwIgXimAnVdRHrx2HwE2s%2BOGMmjlnyMrCOvm8L3W1ZglA9MqiAQIu%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIOVysaWBZ0DUFQ8kSrcA1upcuR3vTKfe444KfVbh4z8ZZc15wHuSHpI7pwm60dtsmL9VRLbSNpRFq3hqj9wTKRDtFPCQPh%2F%2FHKH4Aw5N%2FoYvbFuGknCRcVXJ5g898Q3rbdlQu9LMU5EG8GDXKcduh3%2FQ%2FyfuBmFJYOvWetV9tHrIaCU21PEU8Mc3StBEgWuv6uKnWziUHEr33MpM6ovkpvfD2zb80ypzsYzL1JuJcWMRVchprO6aum5hjkuWu8gsP7nPkSW7v26fZoJKCgr4Rx4b5I4mtOM8R1A84wouRNLZJSGvlPr2Ss42mcAjrxr6TiBZP7FhiUtIFxEqk%2FYp0YlI6wCYEY6z2HNKB6jr5LBAjiZjo60VR3xMTfj8V7zjyl40i64dqGIVhuslvX9u2yTjnGaboj3TOfmO%2BdmIJUZmGaG4lck2LAO%2BrFQeFbWgNETZyrPLQvLuDa4LDGDRd1O%2FjscYBQyN0j8xqtVRZU9YRmMy9GbXGAllrV7uwumgCABbpdhHiehEKnhOW9uhY%2B%2F0%2BVwknFbW2q9ZA47JLAvxe7GIurkECtJTlmKCmVKoaCli930wJ0c0H8K4JWDM0xFpmG1Aq2BTicloENyi6X0xIlI1mt%2B5Da5d0725MuWUdOamvOv7unXkvrWMPOmgsgGOqUBoq%2FSrto9hWp6VXRPbm7QWLO1GxeMaae8eMWGCANBRMmahgg%2Fane5kWdD%2BNYgt4kqeunery6jrhFSs%2B295Zigd6a3b3WtycnLKXG3IP%2FpCqYyUIRf41%2Fen10Yx8wHyZMbO25fz4KXVx%2FvLW8M0pGukzCCbs0Pca4cxLWGu8WT5N98G%2BLcgho0EvxOyVVkFjA7O3kESFp1A9f41MAlnRAVqGmXXhid&X-Amz-Signature=624f81f9109553024c6fb4d142868287361fe1ffe4f7d9743e6e2edb54de9116&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

