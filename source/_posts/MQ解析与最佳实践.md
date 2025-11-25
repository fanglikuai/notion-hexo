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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46643MMB2HT%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T190044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD6QCeNfP7RS1aV7b0NBRZQyjNW7BSeHj56uhf8if9HQwIhAMy86iL6bLnsgXmPiNuvbCkjTPsXrmhSAY0KOaeESlgeKv8DCHMQABoMNjM3NDIzMTgzODA1IgzAS4SXPJ1Jx4Yy56wq3AOMQZcsfrqmht%2Fgqalr33io8SDVXP0GiRaeGQtxYmw5bcrw3jgrTtEOJlrsYZHbSZoyVnFtoNrgg3qOmI7JnV2XGDyJt8dnbWS3zxbZAvURnpOPSMYwr73IwtebWMsH05vwureEvInTO3vHtmOVWM3ah7i6sjZsiDRWt0LNEHiE7bbwyECiLLXy3QeD09IpipH32Efkz9QjNr6SMGx9oAKHCFD%2FqlPsitKuu8XcdnksoCVbiGv4uZVOc03%2BlojQrhszq58OpAo9VDXg1X%2FnGGj9MtsMKRqvmMP%2BE57lVW%2Biipui6ts%2BjOhVaBF1p9UR06BHmXrmV4Gxf62n75a%2F7GIVdRC3F4K61KF%2F6thKXSQbgp%2FjsHW5Y8WsiAvdABSECh5NaV2D7FnNRP9D2JngOhZiI10imNxHgA7pwhXGAwujlBSCocRxladbwtSniWOzdxfMpcI3cVoJoj9p7sdzTPyxPg3j%2B9dcjccFfjv%2FHi2DgLHlVvq%2FaE0Vs%2B6jlA%2B1bRWfDqY4HP07%2F1KKeylG74Nj9YAg%2FB3%2FQIfYN5L76LPPi3d%2F5np%2BbyKJOUi%2B1x02NslY96J9dbCSZKt7ZEP6pg5gRgSl1V95BLwBUvREPEH%2FF6m%2B8G%2FHcAToxn8D%2FTCu1pfJBjqkAeoxTPd0JaohAB1Nxjj8Wp6jXEpEZg15hEsWNHshgW2hDgbcuG%2BCA2AfFiBBZWTHcKbEsaKcAJOZRtKe%2FI6nxiQIs8K93BKxfSY84VxceCARA2bIDvXtAy3KCfm88IFzJdLKDhlVAweHNJ6vhrBTGmfFFyfD9rEyPe7pmyEfwuABnRlBlYgqGHDgcW7kiQHWNkweoDqY6dvDpHSy8FiMLl%2FtfZp9&X-Amz-Signature=d0112ca33f99a73ea73dea09eb6809848ede2d579f0e9826d2fd845dd7443526&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

