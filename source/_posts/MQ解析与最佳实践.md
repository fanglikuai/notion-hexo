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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YWKVEDRS%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T120039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJHMEUCIEa1frbP9qRVmeMYYfAicxaS84W8sEPkrGumLz7NDKy8AiEAz%2FTj7xbPeaFLAeklE8QLLYLldSFp%2BTGfi33%2BYz81tjsq%2FwMIFBAAGgw2Mzc0MjMxODM4MDUiDAqPbqA21fc1AL90iSrcA85kr3J4P5ncC4AH%2BBMHeQ2qRkktgKY9onmWt5O150tV7u6rpI4QhSOTw0W1YIwllaQesRB4xjB7lJGpFs988v37gthlmkT98Yrus4%2B0sCcs1WNiLyr7U27GQW9h%2Bz9%2FZjQwnntkUhRxMsNMHHeNAUEVfyD85IzsrQUGXZS3aE9vWYNhDNCGL5i8ry2bq54dY8GiFednmM1lPaAGFVDue2w9KQqYMLjC8vFlOICbtRbIvbBD5vHIDA7ZLCNyJ7y01sDBTr6%2B82CQpGN9BClQcMpEnXtzB5buWHyS8KG4U6LxE1bYiqK7%2F98H5wsphVyKgHWHog%2BOMWTaPbCyf61V72lTFvhvjVBAgF%2BYa5MoFUS5%2BE5fbZUvNGOnlDhqJlLRHaKGTGay4EOdRXMGaoAdpv9NLVuHY9DEAFrtc9miPfMp51jR8lLed2MlzwrMYzTFPOHKqsFsfFuCdGgV61Y4cD6MLs3vjWnv2kJKGJqV9Kt9PrMIBe2DwW7Bh2xhVyDScIPYwrNQ7i%2FcQb1thtt3EriorjGqHto6oXa83xhEAVyCg8B%2Fk6ic9FvXNuzNDZZ6X6WKlPKi8Ene22yPsf5YSmnPp9JIrNgkUy79FBLbIZNihtoQYkDYiInVltgqMI2c9MYGOqUBSB4EGXDWDPmZBvkMjKqoyzF4CByQr6aUz2aeqsPCI4S5MM01mQ%2BhYC2XM8VUBnBsL6hKGwdRCHy%2FczR%2BS%2Fk6ZpzHDo9pldv0y2AVOalBBkNN4QRBIdQDHLdrbG0L4ypeioJNYc%2BDMe6lNOXYbobFOe79UL2Edzi%2Bmikk2JhBSQ30VQoEKuY9jCTxMMZVJDUTkIqtKp%2BFzzT%2BntRx8msJyQcJ2ENm&X-Amz-Signature=f0042a20f8aa01525dc4f72aa9441d8ce8845b69419e8f266be9e6753ff31cd5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

