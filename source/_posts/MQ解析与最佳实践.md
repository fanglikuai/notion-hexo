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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665M4D7AMF%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T060052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAYaCXVzLXdlc3QtMiJHMEUCIE8GAcJPKlws0QS5GAmEJIDEhYAK0JUzwMAeh6ZTSnigAiEAkC6B9xhuPRLT9BEsLXmEuJit8e4aXhzM0CxEvvbmnr8qiAQIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMurD4fXq4bqCO21iCrcAyT9i7BePllSorrBMc3F79dA9Ezdi9OdCnfSzzvckjcuRr9mLOAxXPcEe0rmLkuec3%2BTuyKNANlfhRDf7vFos2awHvzu5WGwzyjEroVpvxX2m3pMnC5eljiNNBjqFoumwiZjk1HForMoXAUJRnaW7LZejMADcvyOKJ6w3MPNDYanyUW%2BQ9Hz3ld10%2BA3hCqyZnChR0ThP522fDzpOrxrQ%2BIApFqfS8FPL41EhPGvmiOEWJhbigadFpyQ8ebHxZzYAc%2B7xbXdCb683SRnH6k69qGnPicD3EmA48vOqs1D%2BRpL2DYoru7%2FXGT8EzVsLhwpWdkmxSpB%2BaeHzUY2c77MYzohhhEU9lQ7C%2FbZqDU8h1DXTi01PWO2FRA0X%2F%2B7NCePQJwPNWjB3dIU1nVkRZyC1TVrkkd485af5yCW1Yr5STBN4X6c%2B2CFtg5yvFVPDZr389ZNb6Pag27LEehjxLACcbUBNzRyAadFjo7AU3mvCbYSOLsY8SkqWsjp%2BBYodcJy9GktcejZYRudK9fwOGSSFs3ahF8XTUwMz%2FcgbXl3r4ot5oSv%2ByUXad12diGUOiv2B3p3QmVtMPEXzRQp8UQKXyjQIwqGrouxaPY2xYuYN4x8YZaEL1WlDNb63a8cMKuvu8gGOqUBhLVEPQRld3SIBNwXItgvnzEgayxm1yMpbJ8nfyxYPW4%2FN78R%2Br4q%2B6jGFgvLKhjXMkCUeDU8%2FABWdSz%2BSdBkYaqMePhIysX%2F0gL8f4r9I8t%2BaSCPTC5Gl%2BRHNre0JBvixZFH8vWi0Hi1R7QIS0Fs0ch8SdP%2F1m0OOCotEOSt9%2F8%2F2MMrxq8MLaE1FGD%2BGG%2FY5ugfBuNx%2BsV1huk8jujzQ2cwd7nl&X-Amz-Signature=0d4458cf1ceb1917a1c6bc50b6c11a438d2111409634f419813792a44d967a20&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

