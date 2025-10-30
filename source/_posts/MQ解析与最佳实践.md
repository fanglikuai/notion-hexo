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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5UTWZDI%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T150103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJIMEYCIQDPPvryBc3EKriKTBIJxVX4CzHXnG0eNBzwK2bFT6ZTDgIhALZ49kDWVVubzUl1aRckwKhahnwef5EQppj3ZBrRKxQ3KogECO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyuUr25SzAj%2FKsq%2BV4q3AMEHaxt6C26VkGQOg%2BmgKcRv%2FFE5LoSHU1MgaH0rqyqdcDjwKZit6nCnLGuH223Jz4XUTFRlYq5c2ER2Qao%2BXNHQhr09FFmzs1EjL19NQ6GAEc68URFUAPLbyvwZriPtkWMfKpF1rDpMKqtMtY3sRFO8%2BzWxTVk7TcT85OA4pEbBkBaCifWs3srWjvc8%2F3CYNJz46tq%2B96gHfF052KboU4t4KvNn%2FCG1zx0jtQO78x5Tgle4n4nDovzkKkVIjrKW3MzyLADfKZhfnuBFKDjK2hUKhHzjkqlrqKdqgEYbkdA3YhGJIDrYbSrFcbcgWS4P73o%2BsCLxuU9H46rxhvdZxfS815Knv33wqb0%2BylwY1FQlAvjWkubB58cu%2F5p6Nv%2BqMLX5nwX%2FWtrioMpbYBc9rOh%2FH6u4axD25suQ7qFUTIZYxPNuRfTId00tUXZ55MQ2nq2pwm11s0aluxf%2B%2Ffcm38tSAUsfbQFCaA7Cc6QNxsGeTsRxOb2j5T0XGAywb8lRoDllIU28%2BYW9Y5QQS4SK9DU5u6Z78f0J8U4vRn1pNhTVJ0WtadZiFjkLRmi47IBo1MD%2F2ZRd366zRXB8D%2B0lmILAaqI%2FBB7qWlL3k%2FeiJ23l2CmF%2FMgvHpMTFANKzDf2Y3IBjqkAdOIZorkZdte2CzK6iTDr75yEFnVKi%2Bcd6g1lWm3Xtrf7bY1KnCypHJj9V6KgDrmYo7UhziVfdA9c0nK%2BXfwVToGTk2uVIPlnVXtINgEkjYcHyIhSZpWBRowqA9cT2XjroApnoPixFngyz7o0bUcB52UU17s2t8sSt%2BXUTBVVI2YIoRZc2eHdD%2FSxNVl2x%2Bq3Zv1PZrBuL6ZbsKAzmMhO3svO9xw&X-Amz-Signature=93c182f20c7cba9f3989d998cd8969bedc5e227aceb24e76ec355db8d41bd401&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

