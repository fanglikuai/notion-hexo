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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664LPKP2FP%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T100042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJHMEUCIQDkyZWzrfseW2zOxlAXs%2BH0%2FGIzmElii%2B01%2F5QsJQmBBQIgbNv4F%2Fc49ZEtTuxTfLZ%2FLFukSlaPUuXHNtRZNA0hM64q%2FwMIEhAAGgw2Mzc0MjMxODM4MDUiDEiV46mRnn4gQnXsmSrcAzRJAMEN4cBNYlI8afDqAxTsbzhKLe1D8gc25wNmVLtFn4N%2FZ52ZXTX05n6w12uGakTItl4Bj0SH1tMIBpTaywhrOAcB%2BTay6MgnsckQZ5y0lHi3E0pXkZuc3WVFxSneWW57BdLu6zDf3iCAvTv2mT73XKKEDJiiEzitkfekdWdzX8UjYHtsi7DoBrDoD3eYeHp2NsD7%2BRzL1va0L2DMlIu3XjZB1MAebDgPjrRDcDfFYFVTr8EDUO83RNmm3hVlV%2FDy8E1kIQZX7h1whAgK3MDMdm3jLkG7J1GBGrdxzl%2Bymiyt%2BDPqm6sLSSTwdhecoUsDcB6KPKKsHRrSRmlZeAb5EvFxK3%2BHOaNxj8NRsvPMIDr%2BjTpHFtFdeFsnoTfFLW4W4fI27QezT%2B8Wvle%2BhBsjCqy5LCiv9yCNpfUMWvUZz60Te9SGdvAzo9sIKYgtAehRkiHjD9urrYO4ssXIHzUwVSWVItzYpTD7JtScPALbVXe72X1MLiXjxMltHHcRkDwhktfQyHsdOnjbPmINuAwrxa4NTKH1faa68d28mPbt%2Bdt3QiRQxcdPyCANJGlIRZrZzUK%2FOJoYpyBLlVGf80AzOTkW4S6nf3OXGz21RBQCzFjNAGRCX2lSmC6yMMjqkcgGOqUBRNKMRuPIoRwockExnP4vI0%2FbYA0eimRytj9Ylu9Ae6%2FJ3fqh1rO40xlzebnnqg1KHquthh%2F3GSxXa%2Fi3jqo9%2B29iBoLUPHSkMDFtQtLiSrPa%2B8FbKec9UYu1NVHSLwGW5TCo4hs3adpoRr%2FavBVIAdFScc2MyzYymVn8onzpjhM42QVUILxVTX%2FGlTxuKm0c%2BWbW7ePuURqLIR%2FOmf%2Bog8YyJjOr&X-Amz-Signature=72337d32b36c4cd9f7aeb92986e1c16b32060c4acd2d6b8f9f7c03a49b6f02a5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

