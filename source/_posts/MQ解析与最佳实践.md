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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VN6YZVUA%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T080043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJGMEQCIGSv2p9ISY9DT2%2BnWdNX68O0Q4LvhTz63xECUWvF8GbxAiBSEky4TE1PuYwc%2FmU8SaZw1UZL2kQnIBBxu4Ft7ttvASr%2FAwgoEAAaDDYzNzQyMzE4MzgwNSIM8VEQZeScQ6SIisXfKtwDCaSqLd4XppS3K51IOj9%2F2lA6cECafQ3hk2%2Fr%2FcMy74dLElLnXhT82uGZ128ivzHnRZ5B97%2BdsWk0yN9PdlrHblIAMPwvbriMdQ4zXCS4TKu5jtuo9WHNhymnypPTbSk0IbTjPXl3qb1B%2BYki5rHBs639pqFybks85J8IWw%2FDKihZd2%2F89CWk2bzaa5mYXiajhZ%2B61FCTg23LukZrJPBYGsJ3jvUQXES4Wtbet7uJSSw0Dh9yjj7FmvferZR7NDr82zAoparzrUKJW5JPu2RmHBA1vlD6YS9QKThW%2FsiqYPuQ63P15r4djMcZwt1LhCfkA%2FCgQY4uuPhW5U79M%2F08C34UOonI%2F8xl6UY5kApWwYpFLJpAG7DaCxmuplbY0YmbI9t14IWrIeB1CKVMhaRn7lUxhWSjOItNxEcJyErUCT9GmD8OInlDIE5Ci1q5rBun7uKW59NsfagvBzbBL0BZWs32V5xQ5FeMRk5Ik%2BsRsjKfOZ3BwsXDggNhQFRGQEprwcM38PsaXGffJFd%2B84V4rNXCT7jq1bRZIGqKDGOOfX5%2Fr%2FGXwZPpU9PDlOtzo3%2Fjo%2BYVRZGx%2FaSI97Lv%2Bom%2BUC9cUlJqJ7lHK6GUtHp3feweFRczMJfFU1lnVE4w88%2BWyAY6pgHiMy6msfECk4v1VZby2JvzY56JtdXCn4H6aizhwc62sE7CwK9Z7lRYVwYpt%2Fpl96VNGTYuS1H2pYCw2xfywE2P0wMhnSnkEJgv4iv7lbSqqgzy1LiiR7Z69jvNaJeSYcHKAdLsaAVO8dAlpXNPDFGwiTSI4YtSOCNq4pCZj09jICWWbVeBn98iuC%2Bsgww6AhP9oSswWICcoMfv%2Ffu7Es6ZIaT%2FhGRW&X-Amz-Signature=3778dc11a621e2e49573b92e61bacacbaffec6bd79ba644b44d56eb9840bded1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

