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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662AUCAKC2%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T130053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJHMEUCIQDfCDLG1leuukqA1mCQbRejXYhl1aicE%2F8dm9GwA8DLgAIgZNG4FjLIB9KqJKi7y6G5mp0LoZc3TZ45tH2mgC4WtSUq%2FwMIHhAAGgw2Mzc0MjMxODM4MDUiDLJImjtO3A3wJyaecCrcA9O442gtV%2BZNtwE6cWzAF6iiSV78%2F18mbUceEnRV9muw5eqNLgaaNqpMpy2S%2FZF4TbmnYCqQ1YtOwoTgW0IaCTijHIK5A2G1TXXXdzViHFG5OKPTIU8GnLAe9vgmqxweWjFN4k4xnBgfO0oE8WNe6FC%2B5FGNoDAbtZUupmbvxig55gRXeVcw4HpW%2FIrS8tEwAZYwhWGeEdnMKsj%2F6aE0doc31VgBKXwhtrE0FSS7iDRK8Y7y0EEq0kcQh8MWTWUprajZb2Hn0jcNkMD8pcBhYtlX5qqzC6CipbPjxWqLHIpL66CvXQJ%2F5tjJhMkVCB7lMfn1DSMHJ6o%2FcstK5%2BgDkbhT5HZYUDXwKqgBrj60tPXaMbccm0NgjYD2ZoagTKmhvfaF1eahJFviy2xkCegx%2BXsioZX3vw1F5no7G2yCZdDNbSC%2B9ryKwnjSgPj%2FgSSPmGhxHqw84U6Bj%2FIBAtAbXAdjeR6MMP1LWYR3DvsI%2FHxMtQSa2rBqWSwsD4cuBPcQuWNhASsEY5fHOidHIG%2FP%2FWgtOhoMvpJbsYebnwO4p841zqIuflqqXoAwx7d7f9tAtfHEH8kH0UAR%2BhSLpxlSL1UDSUSJRTfemQqhlI%2Buf4njPb7Gb4NBkrArBjaFMI7XzMgGOqUBYHUQU9zIVe6UJp%2BQ6MG2qqs2GqUVm0Gq54ewKc66pFTR%2B7SUeSZPyfIl4sfVJyonWOCj7m7WHYS8p8KLBImmY8QhIHmJ2Bzhh%2FngUU%2FxewY6eSLP%2BoTgbjOLeTZR8Bq2ev%2FKfbdCnKzG4hZ3LlRKbIcv41iEJkmHL%2FBYMWrse0osdPh%2F2zLx%2BLHYAexPgySvH5xp%2FpUXwoZsKSA6%2BpuMY7Pg8gnq&X-Amz-Signature=90b7e8b5c57ca620a72e3f7cee1ce685a44f461a7cba9b9358e6b2af16915e9c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

