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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UEIOB7CH%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T230045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJIMEYCIQCzWDEY3EyqTZlbL2c1jlfPApLweTsnkYJKy95LdDpAagIhAKyLt%2BqSaGM5CrdwIUQ33C4BiBaFHAb%2BCqMIR0zVVzX%2BKv8DCB8QABoMNjM3NDIzMTgzODA1Igyd5hhzLA4y89Igeysq3AM%2FybRCH4CEIshhNSJFcQnt17dMDAeEgxlwBUvnTG%2Fp4Fi7kIGDFuvdRkPRUA98d0SufOboHosHez7kqlxyB6eki97UrOm5Prc1P%2FlsAuoRc5385zDzsuBmW2W9lHqxYrHqxk7S6bKMWPG7X02Ln%2BW1A%2Flf5cNl4ksoYAGYdfg8vKI1mTZHJ0epRgErWJ%2F1tmGJ6lRNGdPOa0g%2BB%2BGmxjSyWp75JNBEenUlmD2azF2WTFVuWToy96r1FKi%2FWvxJ73CHr4vht2CC9F17Qwg%2BIE05LgLZEuVUppwU%2BX4YeLuP5%2FkQd%2BQp3amY7M94FjDfdpYJMra%2Bntx2RANOrt%2BzY95u0%2BMA4knXbEHGzQkiVEWncDJDRAyx%2FmzRnDxX6XdL713xM8S9qiSzrdUyeefbuLDuhSqlCWWtk%2BH9siwNxV1D91G5GopW2BoBaDDv4onNOQ776KOJu9O8coSN7mQb4Rx6thJZxMoiw4YxtHtoU6r0APYxnycJkRIN4ri2hwtSQ%2FSovXhrXIinQt9kDx0qq4wx%2FRWD3BeA%2FbNaKNeOtexsn5mTtDc4xucwrb1T69nLi9kRLsVxFFq1izE5h%2BMpoL8cFMY4Q6d%2Fy2Pgvmo%2Bbh1flV7qu%2Bx3UbyfF147jTDfpqvHBjqkAZrB7Nzt0WUjkamo5xjy1qY4d18v815yV%2BiYUZGjL6vGD1qvETmt0uCgV91ZbpTBQRV3w1al%2Fz20FHhUxpYBP9rqRCFjQrKTjUD7Hr1MkNOmky7WX%2BkFOD9Xff0DFOB7GPAHsOwSULgkc01YHaf216cR5arnROToV2joNXYGJLPo9UMym50rE4ea5PBVn92ZS%2BrvyyuT8vxeDZSWLpol64zji3N5&X-Amz-Signature=a6ce4c7c16ed3ba61376fc175c2930aa32758767aac4db0af6142910366f75a4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

