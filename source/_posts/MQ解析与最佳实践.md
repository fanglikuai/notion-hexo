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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YHW57GDP%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T210040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJGMEQCIDPqijqzeC%2FrBQj8JqqgoIVVBS7j%2FVQJyixBGWmRM90gAiBepO46pROF9RStlvn35dnempjyM4gexXFxvZRVDk3MsCr%2FAwgNEAAaDDYzNzQyMzE4MzgwNSIM1%2BiWHw5mYv4bLxmQKtwD66BFEfysSY12Xby2tuzDDva2pm%2B6BqLusabv05TMs330%2BkwV0FIW5yDsQaR%2BDNcO2lsIk24ilbgNDbON8%2FoQ%2FcDEVFLIMpgbotPAfbSQ3aQMHhvySQ9ksDvg6XgMJw7EnX0AvHxZhLlaIMehMk9lnbzxAI5%2Fc94zkd4tTIvBlgtjm%2BGjq1PLCmjnx80REreTc%2FbS%2BrQm0nHT8Tkojt%2FUfuLqdv4A4CmsyWsUAdk5qRcX8prL1mS58hyMufHh8YFSqeh5ntORSI3MnjvQh6XzaA6cGE3SHhVW98VuDy1pojvd2AdN9UbsqLP2Y7Da8jfpoPFJcCA8w1%2FXuhHe6lX4l9yMEXJFq8R9jictdmDNumsgzQtzMM1Xuxbo3uiCpiIoSa3N1nvUNfbbGCg7UJExkWXbBrB%2FDR%2BA0AEECUifzlriCZ7VjdGFwFdlCnA6raKPtssXP6tOfEVa8lr3igrhfST5pXTxLum1a5c5ztr4rouj96lMbCMtWYBOte5wBClqjpE5vBRIrGTDvic5xGoeHCKrENLQ23n3nptLvT6CHU8FqP3UYhfdkq2HF65MU9VPe0BE4cEDd731TGmiRtPbd0%2FHNhS8MxtrE3zy83zGn6bdf82Ow5024%2BeyUaswk4vJyAY6pgGAjiLDAd%2B%2F9fyz2ki0KZId5ssUwfuWcp97LQ2Bi7GtlU6l7OsW%2F9qv2BoQ2vTyLYIAX3E9glX3FHXOYmnogBDPM0YpnBOcCH%2BoecUJjSlVGbqBPjcPmE9bcXi5BYaA1zQpkC7fg%2FwNBJ9OcRUQ4TCid12BVqOsVMPQTT9Y75pyiRQ%2FvOdqqges8MocInmw2N7BYvwfCBf5pxhTp6A3FMctiLVNjb8E&X-Amz-Signature=8ec880d7edec6ca714273da7644b0a634ea69afc8ca6422f0201bb2a3b763287&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

