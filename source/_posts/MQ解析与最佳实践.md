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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UHAY5CDD%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJHMEUCIDMZYCBmk%2BSG9TnoKCDYQzaSdQkD1iJqIJRNnPsxBLjzAiEAktKzn3ZsGIx50c5Vdtg540%2FZ8%2FJhLhf5JkMw9SzXlA8qiAQI%2FP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHU7O9H9HNk3y14CxCrcA3o8jxr%2BqvTv6%2FzKr4nso8dVjRYVjqGKVenjVN39cLFdxoae4%2BHFljWSv9JWYRxp2AAz%2FrTMkCdwRxtscMleEWpGjJIJlSmogeHan8aNsqDHjqx6A8mvAd%2BYh72unhGyQcw9yYLQ5H%2FEazcDp5QwsMPRQRSQt0NblJbvC9EkphLsi0nahCbdE7x3ZIrib6IJn6dwb92WLhaIsSH23MY3U7IryAyavbCjP%2BJYL3n7z%2BLq3eDFcHf5Ij8P5aRgsLYKWuD6kB%2FhmLgpvgkjAW%2F%2BuBIK1i2dJJE7mFA0aNvGat7ODkpJAA68xE30mcaf9Iau7DcVP7lthKPrMtQrileGfUXB7Ym%2FWc29yf8u%2BAKYBoRdf8zCrq4GRxDt%2BT4vzE%2FIxI9k4ug494gVqaZS%2B2kRVPCRsppgS9kpQtYQEtFhG4%2BDeJQfJ15nMoxMB%2BFWYLdAkzaGwAOs5peDtwDHhOIxZ70xviUvtic%2Fps5InoyYxgSShp5lRahIaiIIz5%2BKiymbClQz6uAlw1VwmNikOyfqbeQX%2FYt3LvkVdtzpdN07n807m2OSQvOyTEiPtGi9lbBsgEnC61DEfoCAkIPlOqMdSaBSij1Hxs%2FAHDX%2FeLeIKB2dGhGrmLimVfUVtrtwMLjq28cGOqUBzlfHMEnKqGzxlNxKDeKrTFTCTvaRLLSJxrpckTBDDcQiCFasP72oY%2F7K16rzoOH%2BtywE%2BeK3cN6Jd4n1%2BKaRRu0Mjao6pmGerWsGRIfOH2%2BwthUuRae9aGvRwGlqAkvq3DgmSc10TJCB5HX0YlQC2sbDZEh7JZkZuKDFHXZtmUW0FPgYkfN%2BY8YG8D2W2CkdLJQnOJ5BrUXUDrDTcOyMQ6TWv1HS&X-Amz-Signature=b70f57e3dd55994e978c4489cf4737bcac6f7402b9fb29ac572eccbeaddf0da4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

