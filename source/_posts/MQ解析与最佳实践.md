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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VUUZGBEV%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T160049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJGMEQCIB8c6npNETYnc6vS0syc1yzUnl8QdobfW0b9Kp2uofSlAiAyROLtxaUKe2EoOpId1FGsqeKigpN0xTEDGvB9ff5pOSqIBAi8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMs9gyXkuFggnsJlUCKtwD7t66sFHhVDYSVRVAjbCQiQWVDkfT5AqAdm3G%2BgdSObsoUmaXgLxs7VyNUi%2FF9aSfBqJj3qezKd6mnoqKQ%2BwksKzTYcnjhonbzNSgTl82LtK4txI8zu0N3noD5tOMfZvQkacAExnKPVCHkWNOX3Iy1ZcB0MIRoAvoOCPvxEnUk%2FUg4VkFmVJkNGmyLEmlrvmOD9KlUzJdRdNPKCvoOde30fUpyVNFhqqwdfCy6ZunFobIxfRWzykJohnUDQtvRSHCc3LDpWurs6p8XD8gq%2BqInEaufOf%2BWnwdCuj9xFRCCehUvokYfaC%2F%2BKgjiD8h%2FEyxR4PfekI%2BoY6fYAaAggpdv2Jq5M2KRayruwjS8gpfN1tjZ2cjEavBpv4qfvq443lJ6UJ6Rh5uz5wlYwiHHeYjJxctX%2FajKPPDtuj4A4%2BBWR0Yu1HMmefys1Bw6F87PBOFXkOL%2BbJletMYWmFarcEo1Eytm8BX3xF7kTg7FRF2wm0RawUB7pILndzY%2F6CrHPEptP3wzfR%2F2Hbz%2BOEdXdvSfuNcBhDBn9lP%2BYHeE8PUQoYe4on3XoNb4EZT%2FQDGFavqED%2B0QV4mqSJ3WvMOghwNZtRjozD6nEfuNthG0b5G7xMYfZC5VO1Pv%2BIc7oMwltevxgY6pgF7M0j1RlJZHciRtvVQZH60p%2Fd2P9njIACBy29y30IZb%2BqEHfXNGw24FHgKGGEDIlBp1kP6S%2F8a4BROfShwN2e45UjftXfDQzXbhgjvbe93RUJcJ730eAQNRqrkR6dAKeM5nfF%2Bf1W3lb26laNx8hATmXXUIGm86mszksdwiQy5mIBLxBAFCFagRAK349tgx%2BsnN2khz2FxGYDAymAXdNCDx6Qqx8VT&X-Amz-Signature=80682939c065ac022d12b5131c9a967b983c8ffd343b94983ffa46d5edce4d37&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

