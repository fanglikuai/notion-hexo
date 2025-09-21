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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SC5UAMOQ%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T160041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAzmv99YQRukgJVJtJePeGARe1fv0v%2BpGZKt%2F0WoVKT6AiBrQAjZifJTDB1SgWVhl7LD3b2T2UwZ3hAt6BVE1u8uhCr%2FAwgTEAAaDDYzNzQyMzE4MzgwNSIMjmek9NcEb8CsWV19KtwD2cIptCmM1iATcu8Oq7A2VR7UKsiFADptrRnufCvkdMylbKLiLF4ChL0Hui8KgO77I%2BbuJX0edspxImbG6eL7HxsSF0dSaBe5xM3zn1KWTfZad%2Bv8bBXK6qIzNcCKowvDuFTrYTlzyZ5jVnPaaqB1bqX89NabvO37Ka%2F9KBhdUz2KRyyFitmPhT5kTf3ZJwpFXKlYo3K43yx4i%2B0uv52lAxMBQXivkM%2FetD3tE5vXvsUpsK8R5g4KYGPiFybvLGaWT0bQTtHP538KgED7BsuyMtByGyH8cB5tlIdsAd3HB9epnVA8nQ%2FtjbRQOhqfonJ%2BgVkFWMj2CRuSJ7W%2FsW3FIxVcwd7Zha7GsH8gwpxy7xj929Ge1MoLaX33Rejcehr3WXcRHYnEXFJnRALmDPnX3TvBkpterO%2Fu3BHvKs48mlRzf9LT%2BdItBas761L06BVq%2Bvbr7sqTjn4ZlfgbSNZpD4dub%2Blv8itVJnqH0ETG7yGC5FKus9DYykRYCt%2B0cyMdeTY7W2Vnr8vcXA%2BKKkxisQ2JYns%2BZ2iltbtfTUDdmJ2FwVnjMUg0wksQs1xCE286q2d7Fe6T%2F%2B9%2F8sVvJTJFe5ku2DRfq%2BJOZVEdpGqBIRzsAfjenxnqmuDBydAwnaS%2FxgY6pgGzcL8dCJ53NuDUT%2FAQTIBrBJpc%2BK992Bb2aecLzfLIRtHSq%2B9tWG1BxIFVuhvZF9fuCaVb8hXNAFfj8Ypsfs791z9VcyiDy2F2vHtuCvJYctH0sLGMzmdjDzHxAjXaAtf%2Fik%2F%2FNG97AczoYXniMhwm77fwvINVG1btYrkmk88c1eX8ZNynGJgjUDUnoZHK9teMQMh7AYzu%2BXBBByP40HZfqW6SiaHM&X-Amz-Signature=65be96f3e579b25d3588daa59ceac91f6fe09ae08eaa29a3d085213bbda2007b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

