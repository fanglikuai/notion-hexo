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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667UGIZUSA%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T160045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJGMEQCICHnfpqI21EgD3PQfh6FkSVXEmqfA0rPlveOFeSDlTbZAiAmBCOeEw%2FB5sWJEhYDybh8rYTtJHMzCfu%2F3%2BOa7dOqHiqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAREGrXuvHIQMmoA5KtwDov87Sa3N4H7TBFoTuh8o6UX%2B4Pfro2JhansCFaIuLVms%2F4gSmOg00YP2Fj%2BEE2vDI%2FGetwFTpM8jf8eMCgPB683%2BthMnwaLkWCsNnFyNfp%2FR5tYzpFCF11Hmu7NQsu0CXjYP%2BIIV9%2Fv3ZRrb5ZGKmeWCjjMfqbosTR8VtSNmdnNRfSVOtFLI7WLFpYhITti%2Bm78ipkVcXxfEoDLNHMur%2B3%2B7uMVVwv4OrZYBRLFcwGWsYxx3EJNK%2BBzhZQkn3LuA9ZZ5LGE1gnCCFTLNlUuoQvYNGEIuhBtCueKWX1dMCxkHeI3rwOwjNpHOzi4v5%2FMMmoNEWcfEtNAbFfqCoH9Q76YSdesND0aKuj2uHFpQxdbyMcWJcQ65iKYJtaa18KFxYuYv%2B53rLV45b0WOOe2iqpXj%2FhmwE91RaPKZYHrmbyqoVEIbY%2BkuV4B2znp4fstOgzwEjtea5o8qi8J%2FxQt%2B1VHg%2FqOXjFWwnJRgDuGvmRxSRjEU5umYNrhXFjvX2cczv4bw4ebHo83x4J63HF4AEMAn1%2BDmdzMOM4hJax4PqMCBGMAadNf59PiUVZrIWE%2BHL7EYQvwDDnNpxQUX4P9pn%2FeKf7gf2y9n5wbYwOM3eXKKaTw%2BACE7kjxOxXwwi8y6xgY6pgGm0LbHrltlEdfQv3TsDFh1NliCSAxLh%2Fdwf9EAVdnmGtOuILDA%2Fn10xCiBpf8CClPctHB314lqGclXz88yR0Aa%2BkW%2BMlks88HzWROgrv2tUn0Le6xWFC68XBq0UdUDdB2whr2aVUs260ptkxMTQMGfkcaPLhaynAewvOfM%2Bwg2KT4trHWjkPY7bD6j%2F6eyr2cAyauykNBfvFbkiKcR5Ssx2uRVcfwO&X-Amz-Signature=ed38483c49aaf00a5190b97ad1d3a1ca67c44845a8b69ba3967e7730c7f75716&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

