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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZDDUVIP4%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T090040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCID7bhHLPeh4W8XO02GXuhhlkHihxISYhblblvjNT1mKDAiAWz8byUFajbe3fp5szNOYzkaQr1pIDjDNOEyTvQKbvlSqIBAiK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMqwf3E5qGjyjBr6m2KtwD1LkDUmXGraNZF4uBbxV4w9z0nVXd8W4BoaJ4ZRXGVMDMy7P6S90EEMea8XaQ%2FA2LVDaFc5qXJi6ovMD5YSFPLFxUejD3y%2FlYqgjdbH8SFRyc6Rz6SwfCWqzzw35owSeCRUT6s%2FiTzdRxCXob9egvKPZGhkSaagGd5RDfYafUo4xxM3GBJzW7cVj61fO8%2BxDRhDdWa%2B541OfENiIKdt7vbtkhJ%2F3gDBC%2FkTrOCOa0q5VueLCtBPtLuT571%2FaNg3Bq2GDFSCDtcmRLcuhceFjpWC%2B2w%2BQEkQJpCxcuBZ1r19mh2%2BNx5xpsOydXUzbHwP54H3OeeNQSqL%2F6U1C9mPFrHKL6bxz4DHZ1FE9cBKIWwiGOoy%2BRwBmD%2FXI4Wn%2FHavwXXgg7abEd0B455%2F97M2k6Re2HlY19N9vlkP2oYx66rBrhBFFnK7xPnQZVBmi9ANee09SymCSGy28R2xae02UPPUpFxqeBKFktJwDvWtyiXiC8K5syCl3nsE%2FQD2Chmn52Fr13G1emYs3cyMkvKFermA7HCeIM0O9QajB2T8y2K%2Fbb6%2F2IuC805KGvixP5hj5CkcKR%2FyeMnUlbvfJiHfd%2Fd%2FLvPcz418VBwERCyUawON6pEPjBtQP7tYBPyngw2dnCxwY6pgFyCpDF%2FNURB%2BS3Ogqude%2BUgE6eMMn0pCfSPY2mXyP7ISW3RpWF9evMuPtmIwzRbkbsB7J4RiIrvJH0yKsvkLDwv4nyiKP9TVbp23eDPkDBc8lG9KptZYqyAPq%2BnP%2F%2BTFMy4tYn%2FhAxQW0aBufGXH5TMzRfyyFdnOS6w3d59cfKHN1JDz3Fa4tts0rcKNgNTt022M9jg%2FoTCfeNKKS%2BhlZVW31zRaW5&X-Amz-Signature=407c2117c12b9ab866bb91a9a3a1f735dff06d0d650be787c633d2cf40abeffc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

