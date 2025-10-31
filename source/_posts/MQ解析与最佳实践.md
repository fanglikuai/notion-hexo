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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TGQIALZM%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJIMEYCIQDW3%2F7SmNiJ6zMG0BhqXEs9721zboNmbeSIBM9rTYe9sgIhAPAOghTHKIVoZHQN4NcZWcl%2FVWXsUQiucQjEf2eqj9EIKv8DCB4QABoMNjM3NDIzMTgzODA1Igx8HjROX4CWXQMnEHwq3AOIoMIvnZemNmFMr3H6CJUVnw%2BPrYef5e3KaBIbSKlA1rhk18uDc9Tg67OS895DfcqVIiODhepSq8VSbW4B95WKzW4jQ1DkhoQAV4SUq15PBIdBCeZq%2BTyMqDfllnymoGH3Dv4HMSCLDEUQad4VqY9eEr8bEfTmDholh6P%2Fgr51%2B3TTp%2FME8Fq18B3HLOOpZXI04ISzP1yUr9YQCvzc6eVowcW0BgekEqoGYRniPFewlTZfKgOWCdreLr5u37IpHdhzrl%2Bx0w%2BRm5X35htKzWX%2BExDhwjDFqQLU3nCkzzGn4%2FYQZlvB2T%2BeZbrbQJRguaTRVubXtzuFxh6CSJ7I370cy6MRAYU%2F%2FrPYQmW4%2B6xjlgEVpB%2FD5K6CoOjZCerGEHE54ldM1gWrW4hfAvNdDCc5tRsJm8ufA0%2FBrxQZZ51xXOmh66kpMhnCZrTREAiJkiGuf%2BhgBgA7CBzZX42tj33AWk3%2Bvlb1vgO64fbjCQO0qbxjeKbYwciLyK6ugBy6S2RJW%2B5ubI8yRzs8IO4nzn50KZScTqJgDZ5Ldza7FzVielmwan5jliKCEPvgK4ugx2vYV3tfGp%2BcoF3BSCWRLF2mbjjBvPO7eWNcbXoyGKwgmeg18GaAv1zrM3KtRDCIwZTIBjqkAedsqJtbIQq9Nje4qnRcs2%2FTsgEp80i9cyJvIHFE006LqL60DBI1KC0XZnjnbNdS25nQSqxfVeOsdJ3SMxqD1uIQKoAIgXEReXhT33gp8v%2FaZRNqxmCSgUWnx5x2AVATpEA47I5CcC%2BfOOYjNnPsRQbDeMeXXg2hh5a6cpmc8zo%2FOdfRpRfSVzqcg54PUo6%2BOKdtgHEdY7Ge0mWniYBFNmWrMGQ2&X-Amz-Signature=77e5146b5d04142a23fd202e8d11dd4eea04c250b7a18ce6989c912f76ef5da1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

