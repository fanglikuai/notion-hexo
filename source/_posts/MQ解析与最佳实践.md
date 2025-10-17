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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S6K6MPPR%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T220046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAYaCXVzLXdlc3QtMiJGMEQCIGInNWSyW7qr6KRtdOtNexhTWrPwktpMyJsHvHvj2HGAAiAn7BJu4sakucRPYhvsKVifVN5NE2Rc4pylZxuhrsccayqIBAiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMFMOfTG%2FGLvnafB4NKtwDo5BqB7PS49HFd2oZZtfTSlWAH1LEn5VRJEHKQPCfRwKWapdEquiJjT26n2bInBYGIRG5R7gtyiLOxs1WkdeKB05PoVeUYs03ejVyhRTJimnd9aH7Musienxg%2BAArCiu71kxNg7TjP%2BQZL6AFYLR%2FeQM%2FKoZvLAScvjcW0mkFX8GiTtMe6iHtdMGl5OKmdECmh%2FRJCeOQ0F5dFryHrnrh%2BcB8Nqf1hFd0jwSCz0CdDjAYOUCJ5IpEhvUoa85Gna7v%2BGw8XkV4vG%2FmK14oeVDR%2B%2B8ieMVOUTs7b5WFGJ7zkg5MT5Dm3eIaLf1GfBi3ojwlUcioN8X3Rb%2BekqVCbL4q%2FDhnLGDKICT6OzK4Rat5%2FaBlXLvdgDj%2BOWWpYOOMKGa1CKpY3uhl2BCzgnWBiKTCGQhPZ49lWn4LdhdOl%2FMPmnFWAuV0T4plELpnliE48bYd5G%2BYBpp%2B3iV7ZMz0U%2F7RNcio4tC7kxwQPJ0RpSZpfDXvePP4MG60oN7iyEPyHXFfnFWfLpDeXnH6yf6Omuw1MJXvQG%2BN6mRh1udOH57DfbDo%2FYi2oDmaRga1l6wmwnZxEBqJT%2BSUXbqFAiJ0JZl27v8FJS8Ec1PtHbb5OvGIoGaz1bSmKA%2Bgy9tVjgkwhvjKxwY6pgGKhl%2BBI0vDR5HaOVSdS40ScIt%2BtcHJ%2FhiHz0a4RzBWFTnZPlv%2B6R3yKiep0TlJVSGntfNstSScPR2KafzhTELZJakuJRW1VYy4VUBbOEJXXvKNYsms%2BFmlnocxpSKMQu4ylVcD8mow0UF8bLsYJ9wsal4mRF4i2Uhl4ngdFKtGatzBRUsnSAjvr3FHGePAH%2BEqsVdmuJv10wo%2F1sGuovAKf5WXNd8v&X-Amz-Signature=689f39d9feb402265c7128b0bd6794f853e8eaf0ff06ed761a9b2e6faaddcfb3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

