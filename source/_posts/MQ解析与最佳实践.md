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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TKH56EXX%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T070050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJHMEUCIAiSxzsK%2FGhkO3oha3aI0giaptCG0AcexDXiCPS8LY8XAiEA%2B6nx4MGGugSsV%2BP1YRkQhARTE9MCwL2eoDjG62PhNr4qiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDItIzoIT1wP1yNJzFyrcAywu1%2FfjFyEsk8EpwnBJe19FUZQEVoaxsJXOFoy5Yn2ONBw0GgR1wh909kqf5WJH40YUwISGsttioBhOql2nTtevHMXY0xafhRCrWM28HCGHwA0MRT1sok2%2F2UgvpQi0%2BnNfx8WJ%2Fm4suo7xngQIKjkjn5ZbHqIlk%2FxHNp3vttVwc3g42%2BAaOWf2cFxtJTHWXQPP0xbOj3voEPm3cOnmADTULu71bxy2aVQWqK5%2FaI91R%2B7Ct4PLmz2R1wngTjIRKMyo4TEjnopNaaaz4A0X8hzQOq6PrSQP5nvKP5dxX%2B%2FO4pozTKJlxPymfA7seG9ONXUJTFyiMtRYHsLDcarcVgy%2BTQxQD5KAlnMzg112DYcik%2FRAAQINM3DRpI1L77P4rVEJjhYdTWAlk2O3YSHjE4kvOET%2BzCaVxreDG1jOPEDNFeWr1D2AB4TxEBwkXrxHaw4Dl4u8on44AlPhGhRovQs4YAjHTsHxtI%2BZjJglOroLVuvg6%2FMgQEXMHhB55T0FTLn7C7QfV%2B5nLN9Cpuob8GVRSi2gs3TXJsKvxze8mJELHFHcCzo8cLduIVFHhNwsjnaZfx6Akm50sqHMoMjc%2F5dFpMdUEJsayYt5TaFjrHw4Gh8mRS7rNH0bWqQCMOO5rsYGOqUB2CsKjyn9E9orfBbseViAcyxbzkJqgs7qglw1eDpRu1Tv06nc2dtWvYcBkF4tDuv71yoYBdFbgpkscgZwHZ%2FpOk3DPZzPNfKq%2FUDWWQrfszuv3U48yQIUpMrtW%2BzoxkfCWOiRWHFPRq9m5fWo%2BVftNeIvQa0NUv3h2cvpFRbcCavx9MsApvvwBsjf6OF8mY5GRYehEYgk1o1p8B2Ec85u6HS9jyXS&X-Amz-Signature=a92ab2bfb78494a82dd724384fe088f141e5678d06d935ca04232eb049e70448&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

