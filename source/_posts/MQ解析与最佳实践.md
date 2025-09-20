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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RMCWMGXF%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T150041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJGMEQCIFszqLMW0PLdIe8xolDZ8%2BUSCISfCV8uYx3KBzqyiTSgAiBUXAfv8JWGFJXCYRBq%2FDj7CviyTpmhsAlKmJ0toToL7yqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMIQtraVSavlYORWKvKtwDXac1J0SlRH67jIbiFpA2AXsGiv8CyzJvoscopkedHLjA%2Fl9uqtGoei0lFhPhuIYRVNEFlxA6nFGF1I7HY0ZprNKqrnp8%2FqHZUntIEtKOfDFXwVXCTuFpfoGxM2TlxtdHbLor2CG%2FMfTzmNKoyYfw35d0mH0bEVNiRIHZIKZTG%2Be6II5JBIcoi7PalT%2BlgAzDZbbMyzRL7AvEQD6bVQMnU8tEHlczdv91O%2B0ipBVP%2FVS4vOx1bOzIzvQg2Fd4YbYuW%2FVdyetH%2B1U3fcHOVbNwRjxvA2xYvxPbq6vtbZridC%2FtcKG7ZMHCIEP2BcPnzsQP40LD3RF%2BOGCrWyAekTvUphgywPap9mkGzNvsctCHOOu20cWxRzxa6bUy%2ByvN%2BaGicBhq5o%2FScMEzuePwZadmAHFrhpPLRDkLlhAL5syuAUPc1xILF4jCRCWdc173txGCniMkiZtvACybxd%2FXB1ozum2VcuU%2B2OEEevXYnjWG%2Fdlds2za3X%2BN%2BpaoY%2FRygxgmd7Q0I7qBD5Y%2Bg9I9y3T6qf2UsfIomdQjl9pmB%2B%2BhvNIijzMZfRTRqWsZxaRmXE0R6HkU4LxUc4AtKA5FIUzaEd3bRDbvXHEw9vznTxIL7259%2BGWiqxM8nLX5amMw08u6xgY6pgHTQEzfwIpsfG1RSd0XgmOCNchvMv3EyfzvvptXUV5AsX5OQ3GToDSTxwyXLBORZKlmaGT6Ebz6sNIruSrGfGahwWCF94OkgeCzOL4ZCS8B3uh0n%2Buj3dpECZMO14Ht2zVRfd62%2BGqGMXFgxLMGkM68NV%2BbeWtRIk4soyp5S1lOcRwBnnMgORk8zA1KBTkzPbqrXKVAe1hUFokzt7lUez5OHdJE%2F0Wp&X-Amz-Signature=c68085ef63d9baf60c57ace2b3108d93d005bf38e9becb0558da3fe15746b85b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

