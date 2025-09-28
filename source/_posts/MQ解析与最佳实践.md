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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RXXHR2NJ%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T040043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJHMEUCIFgRhKFlk5Q0gDx4D%2FPPU5%2FSJTkMc9DrItT9fVanR3pXAiEA2BDyTIEinANyqAAcE56shyrP%2BRT4MbNTlDbmIEVfr6EqiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLwJMf1%2FT0TumRWE6yrcA2I9XoaPuNDypHOFDM4Q4jzEL4TCCDf5XSlWrM%2BX3UdxUrEb%2BrPgJO2%2BYXX1WIeT1h2wJTSjosT%2BbcNlxtv1yMas94CkguWi7hZ6edm%2B9grLLpEP5uyNDcyAK1XIyWS6PeMdMH2wTTWxW0URajllGosXAYr5lGeiYv57oj%2FLLNXo50%2BEiBLcdcUmBUitCdjzRGDT5YfJT17f6f9vooRDr10zYt3sifC1Sv5yEBwq3G%2BFUXfBNF%2BiGqfwN8Gi8jHyqzBB6i3jhMs9ac3oLYHlnQm8eeJhGuKLVZ%2BYJowA7HSmDsZld7EWIGYU5GMjWkEMEnpaIb%2B3Ul04cI%2BbLZoGr1m25lISbzUYuJxFGK4%2B4aniE9k4X8FHbkyFfaq8U4F14EkYbUSNmTNF1LO07COSXO3x4XrgeZuNLYUkD8Ayj69bjN8hDCg2Y2Chhm64MYcR0QRLtxM77Kgh3lyR94co1ZfTeJ0f1lfRqw04WVnQMMCavWC4CbooZp7Aa7%2Birfv5i1G%2FNEsiyttvxE2MfgHbx24UkmvWUoJiXoFJYzXdalJsKwUhMB1J6VGnyjAUX6R5yL2y0Oa%2F2UizIekypyRwta%2FntKCeg3Iij%2Fz0%2Fujx6h5zLU9%2BmSav2rFVrmlNMJua4sYGOqUBgma71E6KstftMR6HxZiix8%2BKScMDgqUQdcusAcbI8dC4yJDt8xie2guHKMIfoIf4sUT2mkVg6SAKitviHqk82eUlt4EW8DrAoy88zJRINy4kA8tvd%2FSaLDlQrcg%2BnIaAbR558S4z9sa3nZF1jjBl48HdtG3L%2BISJvjTkBJqvRoPjQHg225B2LKCBLJWm%2B%2FUjKXynC6z0eH%2Bs%2Fxh4Q%2FEByC9P9I1G&X-Amz-Signature=4149fcd2856cd187efb26a982cbd1082cd551abed4ad98810381affc3669212c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

