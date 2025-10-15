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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YWG7ABJN%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T020048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEr%2Bx%2FAbCKzez4zrWs%2BlXQKuXmDFjKpjKg2keg6vgww%2BAiBenvqF21BU4TRDMifgv9lotxYta%2BWEIfjrgH7f%2FiVphir%2FAwhqEAAaDDYzNzQyMzE4MzgwNSIMOzlqRAm%2B1T7XvooxKtwDGauTBNGs93JQqvpurUdmG1%2FjZ%2B%2BhO7MeLzVdI1NVUEhoWHhD6dZ5HhuN7IrAZNpYhKYi45e%2B2iHaTgQNVA848rmvbCcD2uIr4U7jBiubPwPClN79EfonBKNiwWWGGiL53VeoxVSChw1OrK%2BRSdZ0qB9oGcWTSxRvNozMS5REibQtkQxtW2UVGhgV%2F8M54gl0nl13mTmpToNcJIXv3wXPq9AdvaUnr3avl5z2INhC5PLRVLQ5ubyN2R9PzwZPGRUTtLpb%2FnOxny3sakIKqBP%2FPkjbP0ZS6hlkjJaswWTK5PmjrZySJIRCFFPS0CAv1ZOuTPjbyHloWNVYk42aRTAQvmUH04re5T%2FzBM9FpWHlVPAVbz9KivCnIz949OsFf9hiZN%2FKtadhuEpPpRtUpRzwA7FbrAOuGiGzJ49d%2Bhkbqorf8INiy5a4VKqTrOvs9oPT%2B%2Bg7zRxfsnOSxpIN0wHPKomWeqGy2nW%2FynCxJsZs9M4OE5XrGeprPwzOfDbLuKOgLG8Up%2F59wjmHZVngZbu8ZxE%2BawmF9ySzVW4yeKyl5oA%2Bl4XhlEGtgeMwU2tOkBw52pL5eNfuGI%2F7WGtmJIIVHU1KzgPpRp3llZXr42Xrj4mcjnkoWpx2qjLLOKsw2ua7xwY6pgEY8WRbCUTpony3utw5f88CTsCXCGyhPyMEVlNM0iCxL%2FKk2HXg%2BSW8vg%2BKjyNOlhf68Rvoo2HWRKYaaUW6V0DF3pKlIlmJbc03fV1MCmTUoHkmxahjnmTnFD8AQEGqhgCVuyIIFr%2FeuxFju4Ylsp1kdJrozRGspGELnT03pnUEHWBbFBdlT1PN713hK%2FPU0OzKND8t7%2FiGACx2X%2FdT4w%2BKhzUmxlQl&X-Amz-Signature=1989450d770d37d16ec2b35e9c828cb8c759a0439b90c66de12be6d9526de4ce&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

