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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VEVZZBUT%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEUaCXVzLXdlc3QtMiJHMEUCIQDUECtOvawN%2FNG%2Bnrkweknp5iKY%2BX4ZF%2BKcGvlI0PsCIAIgQU1hkb4tVtLYY7ph%2FcAJNaGQINFTbOIWF8iseIEXnJ0qiAQI3v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNEqY%2FbKGQu7adOPyircA9nCBwi62jGWdUn07iabuzLCT0wKISAPq%2FBqM99FVTCSDFh4iXZKJvRxrJZdO0nnuUrREZijOe2X%2B%2BXmsKMX2c5O6oOevEwLZmp6z%2B%2BtCXDHC4Iqx%2B4WD%2B3%2Bspp3vZ8E%2BGPhsamsQh213qtlGOHpg%2BDVYne2P9cR61xsOUo%2Fa4saP%2FBY5ahluV7s560dsZ1ioe%2B2rmboEUp5M%2BOtPjFZcamGVtsCbTnr2vCcLxoVbyYhJM5UPmc%2BHqWDocRWMvoErBlygamppc0SANquLXq9TXWEyIO%2FEqtxY4sXWr66nHBWxy8U3rwBfaUhxiLaq%2BVYE85ocALf5xToACnx%2FZrKmWVz4jzkbdP1sxt82AfkNvtnnwqcz%2FPbepr%2BuURp%2BP%2BRtNSfOKtFog03jImsmEqTDCYi6ZwCXyyfxL0TidMX6dDT9n8Uc%2F%2F9Yw63M7Ar5lf7eLeG3z3tMXDWR8sI79Gh12%2Fx0XqRW1%2FHc5y%2FS%2Fl532TQP%2FUJ3a4KhphTib1Yck%2Fjew7qs4MQ3n%2BUt0BAnLqMtUBnm3MK3JDea%2FaQvKI7Mgz9KX9ILUlIgnAI3G2NKIYjNh%2BFp9tESBpiYbCX5cArK9fW3umFV4TYvqHPu6h6syETPRm840fUdxRMY3SdMOvFoMcGOqUBsSUZjtH9%2BnPffN9Z8HUUgZlGveIk%2B%2BORMr4SxIkX5i%2Fg%2B9drYozdwbvG3iZZFjqvw%2Fm77F15m6M0Wc1PG7NEMigWQVmPn62l1X9%2ByO4aar0MiaiRcvQ6wWBBYXn0Yn40t1yXD2%2FkdMI9DgURXEgr5BUP95NfOiewSFBNTqT%2FL6KTeQhmzLSzUEX%2FqHbMf0Ef630E%2BFyWxaDFqU92xGPe%2Fz9Tcky%2B&X-Amz-Signature=e1801e7013a4b1e6b270d309a9390f0dd54ccd8aabd37b9e924c0e2c935c889a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

