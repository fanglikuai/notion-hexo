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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z52N2COV%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T050052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC0aCXVzLXdlc3QtMiJHMEUCIAKAUjAmPzbtgwsylJsPKzSwG%2BBEd6Sv3Mjnk1wheW0BAiEAyXQOrnwWWVd282PNfR7%2FUi3CdmBuvYe4ejvUn%2BXkCTQqiAQI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAysy5gjNIKpZW%2BRcyrcA5F5199uJn%2Fre8jy7Frsdb32SVBaiHgm4G3ADayfe2AP0YdzZmSMtBwEbLPYo16jgVCTDgpM4qtsjEtu8RVkdtOmuIOh%2Fu0bGRuYPDHwc4lsb0Bl52%2F4OnE43UTeEHT39XcfiAwtcAjnnoXMJyj7wEw6H6LamvM1Ox57uX5s8QwZlfDyktYzEOKVq54Zxjlzc%2BSgVoExSanR25cTE%2BNcHFhiw4RRHWvw70Rmfto1VtZ5XquBSj85z6jzTyR2ZPF9Qh6QvXSSN7bBwDNgFZoUv%2BbLa4nM190%2BmgBgZTVD35xAR%2BThomH0wwHPt01mTwtOarVRJoG1iAU6wKNrhkfBQd03frjqKiU1O6lFpQ2ynERCdsnsuzZJLEExndM8sypIue74c42FlSZEWLFftmtisSc9r9mLt0rJV7Hm7stCWFrdRc0k68YYOKDEfgiITezSkJXy7Y6EJja2ix3FhJvu2hG%2FctHjl84XeijiHHrk7UhOM0U%2BuH7HOuvPry1MNnuh323MKcDmwaHQ%2FYsAsszXUX2UogjMt1J7%2FfChJpZ%2BZ80TJNkJG0UQRenlg1y4KkOPorzB95lKGLH2ObLK5ZC%2FZ8soJ3aCJiBrmieKMecKduoieVGew6Mf%2BL%2FvbgR0MInRi8gGOqUBIUuZTl7g7MZ9JWURWq4dYNbyGr6ff4VqxSM9KIS0tNN3SUtoHuwfwX4fXayXK70RGzwY3hbs%2BjtBlrRDf2vEv4YtmSWnDXxh1ie8wcDUwynA1YNygRNyM5e0GVK%2BIVcw61Nb%2FlEm6Prbx0D3sytRj4Y6KoVaJkNnBrZQvItm%2BALJhN1OPoI2q15Zd6%2FiicPIHnqa94LiiGtt93Noj9IbdX2qbxSa&X-Amz-Signature=a52a77443df509b6db74314abac600777ec8d221a99b9de9a412ff62b07f411e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

