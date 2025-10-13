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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UKRRCH33%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDLq6uiaffAbGph6V3zmxj1brYn8zf%2FKDxhX%2BDDM7BsWAiBVO34inyldSd2Gw3gRxJjw%2FxGGpXgyzrbqqXc9w80Tpyr%2FAwg8EAAaDDYzNzQyMzE4MzgwNSIMltOq9ejjf7iJJpDkKtwDAMw50kBxjh38PBP05a%2BRe%2F1zrhBquQdfPg7ibNQ5AcoWVgwbmtLLwnLTt2sywAFO84FcNl9IPtK19fwB5wAqH7Sq4bjA4NCyRS%2FT0OqxC%2BEbmGi9VGZfXZ4qloPsmKWxXn%2FV0sX%2FMboZLwB%2BUGRTaIVNcqPAcNfWWwLOe455CafQaekIBeO6yK589PehLoQqrm9kcc4bfb3cvcUaxd5NW8rRbcDn%2Bo3NBaMtyyy75lCZUOEEjshU7H4OQRFRqODJbXSlcmJqUlK%2B78wx8x%2BlWq%2B9F2OmnckB8kK695UiY%2Fq%2BzII76kOJQAgJQdIHJnAaGqNLUJLdZnyw4SGYZAXk%2FFgwTvCOW6FfpQbGp460aVfDR7XImPHh4KEHUABsAbMC%2BJWOz%2FGF5KHecKXxIMxhYzqIRc1tAEHJ%2FUTzkLniq9Y1vX38a5LOxZ1buqt8oUVLOoDbliisb9Snh4pqCFIBGX%2BPED555EzPkB2%2BUcP%2BGZXNzYXLcZ%2FVYlyItcVZ5vp26n5tT%2B5B83617%2Fnljot3cp9LawmNChviXqt1JKboqRUHNmFp6aOH3foHmQAvrFFuNrxLtdUNgz7XPlW3t1C0C6oP7Chss%2FI68pR3x%2F2U9Luyl2kIiAs774fKwjgwgtWxxwY6pgEO0lud%2BqHBpKSVP%2FBRzFnzSPD7YOMn7cK42M4tEfbJkoRQGNKIclGb6JQ6q5vwHyHCN5X%2FbZdvkqC4bKmM5IyYFsPn0NZryTnX6j1ulhr4MGCSQHfODRwblEitWO5xiby%2BADgo9XRmOiDp9cNrz0azDbkU9sZyhYF5IMxBBKvYknKzVGfi6V5kkgEdsDFmZda8DRX4BP6qUHdxBTOm2ATCwTOXlE1I&X-Amz-Signature=7cb5fd18e5952a79bcf468d19841f31c45b02eadd5a80e947b7998e1bd46d3b7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

