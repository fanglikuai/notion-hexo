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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TOMH4S7B%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T180039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBIaCXVzLXdlc3QtMiJIMEYCIQCAO7NN%2B7FD4z0ud14Wrpn%2FA7od2EaYxmkJv1NJ9aCKigIhAPWJQceeSc%2Bs8eBUVhl7IQNaKJfnkRMPHnPgdeGg%2BC%2FfKogECKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxL7O0HNJ285oAq5aQq3AMuWZOHONKPdgp6Q%2Bb1%2BaYqOftjUxkQMg5K1is%2BbeXwIejas%2FGzid6dSzQlnTMTuPBXZw%2FJchK%2FF60hc%2BpSdPcXGp2lIDO8AAhe%2FtNKldhCWHeGx0I35pWEGuVuBtuIa4U0O%2BNKYD9WKplk8EnheTxyQe6YLcvJuT8162gTRPZmQT4kq6U%2FyvOuJMXsNdMU5A9KXGcVmnkD1bpbCNYRUMxUZ0hXcgjNO677OpFLmwYs8DweJphpt06wu3uP9Uwq4bX11CuAbATgNfwSqZUX6yeYKmurSDWxi%2BmGYrN4VnP%2BQ4EMtpoDlghMIFTyxzdozSFdNBus2N8JM%2BZ%2B%2Bh2q%2BizcSUAFBarrcCck4M0VCui34c%2FRwnmEVK2a%2FTQSTMNJe0kpouc4EEPDigGqc8%2BxC5b8Hz%2FmTHmBaYXlMpyL4VAYDvB%2BGs6gYPecxmWQnywg%2BnOTtXhPIXarrPT6h4%2F9EMt2F3%2BoDbrlOStVqo1YY9OfTXt8mOUoXMLzgfuGJ%2BJKCfcvXG0Bnygz%2FyoOgrXcA9BRildXFOw2g6PwkC0pNqkjGMsp35NpqXYF%2BPFjCL0weade%2FjK%2BzWUA3h0ve0Wyo%2Bdy6CsIk4XNzp%2FQRCKNa0T3zYOmopKk2cAgaQVkBjDNoJXHBjqkARJL2WaWuLPohZnNLMLr8AW09SN2W1%2F2M7prGw4nnA%2Fvt2aERPd7eBdKYg3LKz%2FqLcVtnPrvk0jrdHLnSY%2FvXEnu9WitZJXSeM4%2B%2Fgqoy%2FKd7JW9nXVvjiNMtHp5tM56mPRSltuIeubupNEyqhcAz7xJwR3CzOkGaJ5awwlmSMRSNt30cCOrsIJVzNVPYMnO%2FwCahk3hE4t4JEm3flgIAmctfeCq&X-Amz-Signature=807495d4eb6263cb603c22453f08be0a501cff61fac5aa8c204e3fd9fee3be64&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

