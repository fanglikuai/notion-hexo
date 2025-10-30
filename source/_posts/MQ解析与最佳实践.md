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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RPDSA7JO%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T070056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC8aCXVzLXdlc3QtMiJIMEYCIQD643%2B5fQQ%2FZoJpPXRUI9%2BQ69RFyD4iXCDecFBnwOgDOQIhALS5s%2B6GF7dx3KM%2BMNO2mT%2BuoRSCGwjOptLe%2F%2BJilEocKogECOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz%2B6lq76TANv%2Fkz0Moq3AP4e5L3woeMIlO7wVozVB4HMrgFbZyje3XHkpQqRrpXtMlK%2FbM8JiP1ACUuOFe%2BhqOu%2BhS78xSHA8vtj%2Fb3zEykNhWf4e8UN2opFD1z3jBLz6uXIDUZtnubWvjXQjKsZ%2Fqxm9vWFBydAcS7b5iO44OqSfg6oTmWQrBkp2i7yngLBQDEL7njfl0s96%2FLhkjb18rxgVa%2Bg42Yz17emrlx3Q9yKXU4wxrGMr8P7Qw2U%2F4Xj1vsCIlsjMaCKzhyhJPkvblMspyBWwCNq6hih%2B1VUbvsyICVjAyN1bTQPFkOgbFdS0ALnr%2FrbpRCErTIGNHd0BPPN9cNT%2BqaCLyUXp7Merc74GUFJitSi0EM%2BW9WC6mf%2Bq1%2BWfP4%2BERxlsB%2FqwnxqPfuVQeteHxjKi6dNvwg17lz9WfAKnXX5lCsfIRX4WEReXSpH%2BqIcSnNSNkDrINrOSwBKwSZY0dU3qC29lGJmgkaTq%2FMYjRiFTTUzYlI1VolBdow0i1IS%2B%2F%2FxTxbxlJ545Ikgf59QhiGgiuXaqyjTT%2FwsUNYehCLmlpXbnPC2ZmYPyS7QJsWUCut0gaYjpZpiByBlvkjQx6x0iIZdx83lVU1q60FXTe1dwB%2Fk3y5GAepirFLG9dETD3HPoF9kDDslIzIBjqkAVSjSXEH%2Bua3g7YvAbr2POoeJ6OOYk2SHhQqpkGoXrZBj7lMTsWTj%2FX3uOdl93U89G4VPxkEnUF04lMWeePFuSzmTeRG6r%2BKXS1lThT%2F2xhs1C%2BwTQQ7IGz5UzOu9HnLQwurKO3RdBP3jKXQhDuQ%2F9Xdg2EEEB7V3wONnnsQtGMjowGiiF7sXO0cAwIlQ857a290CnUL%2FJVMVrWCo9StfmgL%2BYOp&X-Amz-Signature=91182bae0a60503d4186887c73bc750035f77fe36d8a965741a75b93ee561747&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

