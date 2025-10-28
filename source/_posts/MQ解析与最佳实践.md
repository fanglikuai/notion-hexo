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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664MCMVIJU%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJIMEYCIQDHkmGKYRIjN25oP6bQNkvioVBN2lUej0bIcNDOljbaLwIhANZgsiTtce%2FpuvAnAIJfy3FGAZmS1HpFOhTVGKE4DLtmKogECML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgztL2jR798ZpLiLNgEq3AMcUsPYCWwpLJuPU0DFwCR0S%2F0MiIU35gzxnlE%2FsVAx%2BTR3ur15QnGwJ7nsEr55rmrX2LaaXcz%2Bq56%2Baf1ZQTPIztc6NOTow9NOBAuWRLBClcKh9%2Ff83N3yfTAN7Rm827H7G33CbNq77JKQob6WTBzmF9qP7RBFEj1aG1Z7w4UA10apAIHpjktNZHArX%2BqVkB48SFHpbLEDwY72Y5bC4yWk84edbKSropIeQU1Tt66g82FsY9vhdv3lKH5X5knmQ1b9d00Xbba6%2BB5iZ0xjdxZDUOBthdO1GFzQS%2BlVLFiCuHjlRDc1eTOs3d0eRARODfrIlksQGBwFa2KlbkSrJd3TrHow3%2BQo4oSOWtjoZmcm2gsaw9bAngHbTQhHC%2FGu%2Bh8OVNdRWjdrwUuwNMauYPZEuJJ%2FAAUjX4CDryOO32zYyF8Ow05dfY%2F87jDhDKkl0us2aZXfuFHoVxTVV%2BrcShOfhEZqGcjvkerWZyo3NzOKH679KnKESVuVwHUkWZB%2FJsfH8rXx8z5BAF3XMibSfh4EC%2FY13Vfj8f50XIPmlQKOhpljfqNMuRKnNgW%2BRIL8IcPGd3cR%2BKAE5lJg%2FGd7lxUnLFy7O7AoekDmF%2FdHFqhJ0qbMRj8fNul8Za6f1TC39oPIBjqkAQUUV%2B2uz1Kufms%2FPaOLyi6QqCYNRzBDhjQ47rskWd3RxvdZWrvsW3weLTnTltnD5SvJ3jzF5zRBIJcuWvYHTPk4iqGMAJxyqlHTJzxf5Ui5WxiEIJ8rNwcZ%2BDqr4v38NLWUDgW6aKU21xzYzsnHPtZAxBRCPyb5coKFuRKYB70NafLZRKw2lmeAaihmdWu5a5t49oMkaauTkZg8512tqUg2ZU78&X-Amz-Signature=8a097e06dc3d4da10c1ce1310b1d1ab94661f927e25cf56dc4cd855bdd6071b6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

