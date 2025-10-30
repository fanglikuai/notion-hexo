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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663DUPKNLD%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T040056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJIMEYCIQCGHnaD6tXpxPBctFEFA9H3EKpalcofdyKKoLEnZmCzCQIhAO%2F685uAuHHBzY8CWoiS8ao9mbGqr2Q%2BUolOKNjZOaoMKogECOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz8RSiGpZQaAOcWLqkq3ANPp41uob71vCJZsV5dFW2Gfv%2FQ1bASCUmFw5hcJ5NAYFtQRusr12fnn1ZPvNm53TCJ%2B4bYc0JXVM7Ut3s6a4VhxL0VimGvZmE7pMw3wp80GZikFvtmjVQVC9riFrWDaVn6mG2g7mEOGWYj4x7uYPcBkA8irAJXRcu58DF6xxHaGmLtwLYV2XT%2BK0xzEi3Ej34B%2FhJ%2BZ4l%2FbeM09iT9KRFo1Gijr96A8rHLhpbOkAnOQQrZTDwmPmuZMrrcyfP2d02TxbV6IZus5Fb3V0sVCmqv6knkgG9sL93y%2FwCK9jlegHYWLjc%2Bz0rgmXkR%2B%2FU9QCUrWSlOmWsp13zyioNiYpc1qP7vd9qZO2wjVRowmSYtYpcBtIDNInUaWsnf%2Brq2x%2BcbLpB8enHzQUGHULQjJRwH1mTomxcSCYkXVgxXVfmVPXoeuGdG8Td3%2BPzci%2FGLG2s7hCzpL9BmfY7skEgD7aFjx4hHuBxgoLGJlfCp%2Fk2rWCk0rI52JDnJg60nmhAMDKqKz0%2BnbeCPrpFHfmdKiDxGiN2ZJH1HT9YXV5CME4GbIqbTAlS0%2FWuCFVli7VFvQnwwtpIQhtRd0lY09Jozi0%2BBYLgAlC5TystnLupKFv3Wj15S%2BcFKmXj0jmeEJDCms4vIBjqkAQZwK%2FggCNnBgCY7RFzEq3kqJM5%2BrhC45H0DE4h4dtY9sOi6%2BBmZW9c7o1ksuIU%2FRtth%2F2mu8nmGu5tzl6u%2FsJNlSSB5haowl75jPMq%2F%2BXhXhFCvKM2oSvH4qvnXcnOP3aRFeVD1hljrXLBVMJ1%2F5JufwrnYUyI%2Fz41yDQCsn2wq0snH1D7Y6MtaqQi6B5fgUJ%2Beqm%2FLGgEqh55RT2IaXoYIv9%2Bx&X-Amz-Signature=8c1dfeaf2377dd6b7f808385ff2ba4673ac424be8be1149e4536f8cbd3001aa7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

