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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667J7NBSGB%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECsaCXVzLXdlc3QtMiJIMEYCIQD73DIAzUVLwLcO66wsx%2BAHVqurBYGX%2FQX2EdqcjZWBSgIhAKoWEwf6khhqDLV9PfAWCwUnJejUm0jLy5yjhbkQNpMMKogECOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzH4f3c9%2FoGf2e8LDcq3AMgtBw3ZUg9ZPrg6BZP6oXRpB8qesX0o%2B0oeVjqjsTt3bSFZBgFmOBME%2BCkO480ffhCXH3GJYXvrxpe3Qz20SAOdzl8X%2BcJuhpbPR%2FmOTPk2RxlGqiaZ%2BWxNGjemKIlj3ZIqOflVcd1zWNgJWxxH%2BEXD97gv%2FS2E6K%2FXj7vtIs02rjq6OS%2BByBbmQrvdMK5dRIteQ%2FXOoPj87ZMKWAJQ57Czw5DYJYoL51bBUHR1Mj3UAmHzH98vNvnCmCuX8LP9VFmuxipHNbcT6lg1FGvl6RoKYJ1vWgzJPHrjc06Q38XWTQhvnnIBdahARRQ9bD1HzN469AYRMDW0%2Bo5urxmAAu1YBgoQ0TTgYFIj0QXWH6eNo%2FW5jZHleXYjYMEKojiYA97WDASHjpR%2FnZ%2BFoptT2djX68pDUJLQMbUdp34bNJRsqFBTEJGL24CNhu1H5MYU5TE5oog8d14lRY2nyqxj5sJqkAVBZYbyxXNb5i%2FTql9BdB%2FnO9XBHmTXr0mImdPU6I%2FNO%2FBp9B8Jwh1KcbR1uePl1bZx7Vd4QmqedsSOVAUVcmo4P7ZNdEDpdW1MPvI4H%2FmeiGkQnKUCjiJz%2FMo562ajLe7la5y2WjEuMrOBvCspP1JddAO6chPnES9lTCxlovIBjqkAZ0sBlmjswc2cWfN0%2F3TXyaTdYxEVnfhaqU8PxrwTzdh89phkq0g90sFa6IugC0jNoHrH8plEmKVIyYNKCeqxZ2CHSPBbmHn28b7Wl8nLUabz2cmnpTP%2BGqjqdGmZdPE%2F0Jy4Wa3mnkR5nnXEVcYnGK4VH01ukh%2FqXFtzTxc3Ih1SxwwL1prHRKF93cEZQCAaXLwMMKJREje%2FwXFcavcfTxRL3ln&X-Amz-Signature=bc29518940886ec13d0491030d9aa9230b33e213a19da5064634a9691e128df3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

