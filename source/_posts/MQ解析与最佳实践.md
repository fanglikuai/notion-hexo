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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46625ZVABFY%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T230056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGcaCXVzLXdlc3QtMiJIMEYCIQCiFjuSe%2Bq5hXT7F9Ij3W33OIYGz09Wv2zzdwWH%2BCav9QIhAMlKP53VeEy83mh0iiTZsnNjKN1GmmFgjaMLlpTSeBHBKv8DCCAQABoMNjM3NDIzMTgzODA1IgzJAdfBODK77NvFgbkq3AMNSS4lAX8iuafQEtJVAPajUVzqdbuf7wWeXZzLtigPgUqTSTQO%2FGCJ6gIJeYvNNNFlw2B297iwVvoiggKKmxZlLD%2F3mjzSUaMdnkydlDQkOEDVJuzhrfo8sHh8ItRw04yKyg2d2pSYF0u6joXkqkd4%2BKxMqxx284QJzkp8vtc5GdtzA%2BMy7hxX1e6SILnThZQWE%2BKjRYNn2Z4i%2BIIHIf7ZSu5EvMtYYNEZwnovSpBmlEyzsFySfVgUq4st3i0IZ%2FFxEey10JfSzlqNEbzb4GRL3x%2BK1LM5WbKv%2Fjcyo4Eks4Ph9QEga%2BaSPHGd5k1FLKm7nMUGLEOTqj2CxPk%2FD1%2FW1X%2FxUwXZD5yrOnz4ybKmrPOHPEUsHCv9ryGToU9%2BIDDYZqXpB08dzPcGD9sAZbTTatjo0Y%2BqXsaEjcvQ91EWOCXgeeM1iIOJ%2FtTcwS2YxTCnfHOgdeQlHSkcIDgGC8OfSx3ib9%2B218s2eA1O7peCJQn1sSbO0Gb1qZaGYmucS1xMYHFhjUWon9%2FRjLqfeCcm1AsKMdwp1NsaTNb8aNxasEjceOezQnk%2F1zE3zOn2I7Wq3lZKtV6BxFXmNtjtpuSaq4KZye1J5X2ssLDbl2T2tLzkprtLBxUhftjdjDCWl%2BDHBjqkARi0Nh66MHwE642Gcxjsg%2FsI1wmWLo%2BwgPnTzDhakOmPtSa4n5KDfI3uR%2FRm3rhBNaYTuvOyAn9wh1Xo45f3R8pypQnnytv7CLpbKocS1iXP0KIDPg2MJuUG0qsl5wobbVKibjWOVMiPOoWWhGcm%2FG3vASf4EHx0kZAlpbIOy4B7ZG6gFWeISFFoIUgHYVc80tfNPX10%2FWIJLXZ6oqsR8rhzcJqT&X-Amz-Signature=dcd0554da104256bdb599b8e86fa1a5fdd41cd48578fbe611a952fefeba6792b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

