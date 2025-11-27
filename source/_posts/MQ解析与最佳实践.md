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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UTQ2EGIY%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGYyHSA3jZczdrRxbVSrfPf3OyoNny683fhgHiyhhnhVAiEA6tcdg10WOCUbPafWHtgeB6IYSD2haVDyp406vlQVHQ4qiAQIkv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOt1LMSfAZih9DO4GSrcA2liLvrwPVt4G7IstTcnytH8238vwC1cyl8cmhmoX8Uusu%2F%2FDWVvbg9TCoIXZ%2Fdo5wu0vbzguIuhGq4JrxHG05fFcRKW9GrGCcUWsnPkM8EFsX4wBxDe2%2FrykMsq4NJHOQ7fxCTLitm%2FgsdWC21fefyZ4AlT9i2F8MpvPOalx4BVrgQ4E1Vqiq8B7a11VXVf1j3Bd8vKQ8jV0VtXl3aUy%2FYDXIILMfnEer%2FXzncDcCPzYVmsbNNQiyha7GSho6yMRn6rsLWsE07FwWo51Rqy%2B3lcDBLJPbGIzPSblqsTmdLeicTRJJ3wOdGoFtUkXwyOGGj8XdY%2F0Fgb14Xhbmw1KDujBqfml1NRpUih1ayDP9eOCcSIuz%2BhobL67VYNzlbwoDQgvUayKBJ9KXMgH%2Bria0owvGy5VIiRRz5xI5MLIMz0maQ%2BDpbDD2EuHJju0BTrwJiAYo5zh49eJCsNFmWXioFigARfPwBliH6YiW01Cu8StIR6rL5j2kVFczAnWutOXBrcDPoRIe9CxY4iCcre%2BcWBLMqrbSj8%2BNhAFphEyvIaX1qzrNoIk%2Boql7Vx0LBjhg7JlTnDY3CF4bDiA7sdgV8HUzgXPaWWd7qxuum27h4hyhSXjlE8Rd9XWIH9MKO4nskGOqUB88isa8XrHbvmlj1hmROapACwvT7d787Yz8pyp49IW0HZhagzjz2gxGiJwU%2F8q4U0oDnpL1AhQEBq%2BH1ftJQH9lWJjhVkUNEgc9dbtYcZYRYNrQC7TZo21FNfSkO1LqO6Nhvqa0W2cdfAcaHQiG44Uzm15VwkAr0xMVGp0LKesAC6hJHzIb6iHZUeBbGyptuqjEIOMAK2ptmMzLNT5iub0Kr8wL89&X-Amz-Signature=96d2d92a04ff7c953919bf6dba26016ce4d4ccecaa8f9879de41e821b76c7b7e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

