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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCCUXM37%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T060041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBWqav415LYSKRgJyQRSrIUniPo%2FKtErS%2F5jBAMy3zlQAiBCwV1YDyyK6f1phkW%2F8l6HxQ0uzOK3gX1S1XuT00TVpSr%2FAwglEAAaDDYzNzQyMzE4MzgwNSIMpNoUvEsI46%2B4ULSUKtwDodENnDiXuPUBCvbvwtGA%2FD42IXmXk%2BSqFn0f4plWElWHcR%2BI24Ie09U7WCkhIeAbGZ%2FEy1802UpFImpdeEabk17%2BoCRZCPiIhXcjpSnzT2gcT0%2B94eVmfwnrHqE6nU0Szt6l46VwQWSidvZsaq4%2Fex15Avam%2FOyDZAbqmRQeykMK2POpLuwKrq7OHwZdzocf0RKFAOyGALckZ2vyALYcLa%2FkPRoqT1IYhzzgFYwAvEcO5bBnE8z5L0WPB6qb6b8FEwTdYhbcsP8M0rRjn7eoomF5YLZKnh7Dt2R2Z0fVzdZ%2FNMiEK1sVJvP%2Ba1JF9t%2By1TW1jzjxxho4tNJzSmwAXs1e3IdeH6%2F07Ge427dhyHiihbYCsYwA%2BVaXHvdHPmkHS644TZNmcJ6kJAD9d0IQBcNu8hZa33LtrX2XFSVv4ThzDfG35AtZCI%2B15FSTziz0%2F3yhc%2BhPWOl3SiE9%2BxxOW5ua7e16zksutT%2FXfuvnZolXAlE062BApP73i5SXPZ1ApsUy%2B0W8PVEmo9TOBLUsFHuM4T%2BHt4Oy%2B2YfYF2524fq6%2FFCLjUKN9wZF3yzG1Dm3GJ8V7S18wroVHSpciODla1bCRpIIQqlPmQjKM7CY66GXpg%2FT%2F6OioC1roww9JLDxgY6pgEZBXfye2g8tYlMcSIRU7GP2ik7vIsdnpnvgUGxc9NnR4%2FOWNINYPaV11MLaswv7k1WDKmC7RwwQ%2FM2ShQHm4Kc0q6ZxxwCweO3%2BIfYXNCtDwu8ahXiXnuCnpSTUS1TbpOnS2HtTnzJMkDMM0pCvnRpn1CJ1niSUBUnOJQTAax0X6O8Jv3Pbskm48lBmJapWJotx4sFtppBXwQ0PG0qJRM7aMg3wYV8&X-Amz-Signature=7bec3fd8d71be1033413198bd3a7e365b5b39a5d854a329c1a906dd76342355d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

