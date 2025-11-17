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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YAG2ZDG6%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T060047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIG2HbvKM8ay5NJKRAR2neWADA3QxZ4em%2FEspgGU1BXAVAiAdZSM7KdVzZH7ugxW4Z9Dqcsumyz2WGni9ywCTeTDRHSqIBAil%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMO2w%2BBc0NQMfnF%2FPAKtwDwqQ1XMPLBE6YRnSVAYM8j3qoRl9rn5IY7wDdnrTRRoy4IbabzwCI53BZI9tN6%2FTjHaupDsUiO8tyElTyfQZd7Tp85ijfL3ysQeK6ttPrFcDU0IqzLFi0ZDozwPWRdeGa9Y77Sugpxk%2B%2F%2B8VVBwEFN%2BRsNU%2BNWfLVCXfAA%2FksP0DR5NDLoJVJ5VkQEw6BjcT%2BwanNXMmM6Q9V%2BCI9LCihfDmlTnC35NEUUQRYcs%2FqDNxVCAOe%2BTM3Dmhcvuh7HlKhy86FeCiIB9hKEbP60Wj9WIH39oe5xfT1s8ELoq31Y4UtPgZhKHrpJsx8HTlFSkMQ8xXK26NLSkCzQL3%2BUn1GjbrPHdC2MV0l3vp6lHlqPkGG4Xsza%2B7hClT%2FVKF7ZddpyiOXjBoChVrFL9Da9PFr9V07HcU5SNDTS0Dkb%2F318p1hwnoZD976XTSKHxDZ5E4stBzjLlp3xy%2BRu5yEJV%2By88eTLf2fNMg%2B%2FPyohy8iEkpqF1JdIUX9JNJmYqch4u%2Bjl5Zwr98dimESpQ%2F9cijs5gbrVQA%2FDzGXPAsicGgch%2Fxg%2FB2cbX1oY%2BDIs76CsND%2FylhEEmpN%2FsY2et68sHxIR1LnKmN1U%2BUTty8wiz36aCPbT7W%2Bn94BMuQtXDowmMHqyAY6pgFqr6EY%2BFrrx%2BBuNAF%2BbangAmYrc70u%2F5e%2B3EICtEN9LgCR%2FxXxVpM9G4gtkddIGuQKaicskYAyKSDXU9tRvJoKHyVqTaxwIsCpVzHllzQhsXPVxYLZn78IFCw%2F5hOo7hpRpqM%2BBAtxt3zJSDoVRAA%2F5e0ZgtReU0PirKY5Dp92XAWRwH4BgHCfpisGCjHogz%2Fi2k8CKNyE8ZTpC%2FHeBLNPJkyXzdpt&X-Amz-Signature=7d00cf1f97ee694081cfc09d7be964217d85b81a7500c756875b6c6eded0b804&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

