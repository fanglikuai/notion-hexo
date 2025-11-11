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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663ENWCXTK%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T050043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJGMEQCIDQQ3MrjUpvF0I0YW5zI6kwT7jGUe6AG7t1v%2FVMDUyNjAiBBkvGBBiUXmNbrLQCvZwmmU6HB%2Fkeo8%2BxJzLnDDdl5fir%2FAwgWEAAaDDYzNzQyMzE4MzgwNSIMkXOaTkB7yrJw0ZPJKtwDKHD1Z3eCuzSkPSCSY%2F7Oa%2FnLQuSV03vv%2Fzyf%2BE947FEo72eL%2F%2BKTsL5Uhe1eSsDU1W8SPMoXm6NjzqhIkeOwV2OcHuQWhuaVLf8bEIqd%2FRaeTjmA%2FksZu%2B68tOCYMel5x4FshxtWR%2F%2FzXf%2FMeUXktKOGu6YF3SlU7rS%2BAoMzo8kV2glmKLRG2yn8fCOHtQtdGD1mf839xpJXR%2FDC5dz4UNw%2Bwk1tXKNaVHsjXcTr98aydT1XjQkV29B710MUseHdQSC3ps25MvphGr%2FXEye4gdTEqg3Gz76%2Bv5w493CY3LXVbN%2F%2BBF3O4TC%2FVi%2FxW3kYQAev2ONrWpxS8iUdA9mrwHICCzj3xsHMOIzYueoI0KuyM0sfXRiK%2FXmTzzUqydMfJcotHNIZzwO5T7H0Yz8D4tW1OLrABVw73N%2Bx39LPQo6ntS9zKsJS%2Bdxoj7Qod4dAvT19jydTWyTnhyZtSgEHm%2BaRdGSB4KRis9O0h9h48jrPOh%2F2mqcvUaA7T4PWYQTRQuk7AH%2BNFBX%2BqnOaToUvCzJ36kL9vqZ82NBwDy7bLIHPgA49YdGHf24PNjXBBDTFSAR7Qn1f7vRQpdr34JQcqa8foNd%2BXhF4FQ0OeJU7w9cis4eK6zmZd2eydv4wr4TLyAY6pgHViQhpA%2Beoq0FYgCoyI04SkHQVRmHQ%2BE4qrM5dsFgLLx3gG1MKS6w4GVyhSeVT5z6HApGC%2F441YZWv2%2BkrY%2B%2Bdq4VA392iHgYeCYk67qBf2DHP1z62BAk5kbAfb9%2BXlK839SGT1Tb0M%2FJeAj%2BKBNVIP7bNgV9NwZV8gj%2BdHEZ%2BFADjLlIdK%2Fet9%2BXxQ%2FU4H2nyDSUhDGq7EyJXX9NgeOcqmGpHoRX5&X-Amz-Signature=b49249c769427cbbb7e0e64063e84ee55a114dc278e63a0079c3fcae9641b2c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

