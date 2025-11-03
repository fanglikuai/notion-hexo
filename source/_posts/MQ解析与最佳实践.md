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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U5I6ZIXD%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T210046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDkADk0KKqvs2wnToCi%2FKm%2FRpSJfkXpgdZn8xsQunOzpQIhAKaQ0JWncJiu%2BAliPLcxMK1CMkM%2BahtFwlIfsOK7O%2F9AKv8DCGUQABoMNjM3NDIzMTgzODA1IgyZUHXeSgt1UgCmigkq3AOgOKuN5%2BhFJ0GXJISXQYfYnlvKGV0WIUIAlVpR8Df7Ld5dn0U%2BfuHkv8G%2BhE4QMjF72OPx2Yl6Xr4RJP7FaiIz0cIrYtcWZfYyUWfK7RX7gs%2FWPrvwmghm%2Fry8qVAf1b8xcJ7xR9h%2FT8E3u2n5aUUy5c0raKjU0J8CGckUPsNmnQAJjN7b%2BTTfoQAb%2BGREXE5iCxQlDI01NfVUP2UkAXFvSLMZVreyUKCSzrwODDULOS8mMNnyvJNYp6VOcHson2K2TCVfWvfluCAeNhclW50nyn0xvKM7gGIh0trBQXc%2F4N3CET9R4vh6w6VGB5QXpxn5EdRgcvI3r9AkZf6w0RSfHk8DtaNPTLihAFqQ2xZhtOHg%2BLinrwHPNkK760KRNynz00YqhridXbTBAvZq%2FqqEPV90gG4Q5tAY%2Bvt%2Br8qlr7K9RKs%2B0xNAWuD%2BDp3EFwFHCvmp8aGd59NRSVGxoHj27RJYZ3uzPYTHuBPXfijGpy7p28o2%2BiITeOI%2BtQofcganiV3AMZ9c7uYKh8GlCEcVKG7CRkbP5sYFG6jccGyu%2B5V8x4xWeif6prtse%2BoSAOuJ6NjOvPKBZZIrZOXE4BdlXojAvEYIWksz07kt9R969e0cxkxqxNb%2BckpdIDCEk6TIBjqkAcKGOMv7rjMrewlFmA2rqzSi7zqNSavZ92OmPJ%2FWKN%2BwFOqzdPGVKlgAlCU1Pta55gyQGPsCyVajc%2FK07ihUyPrbr0jjzEz9LfQ9Yxu5TNSx1I2iJz8avoltb8esgSVTCBRlFDcp2n2j%2Fc%2BKrDqLwXzMs%2F0mJeZqlUSA9cpYTKtKbswaV6hsPVnsTvpg1H2bupPT2OEHM8JHfEX7oAAI8oxxf4q2&X-Amz-Signature=6f43c3cdfc255a76c2c1430617260c3ba4d3e8137340f5f4eaada4b1b8629674&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

