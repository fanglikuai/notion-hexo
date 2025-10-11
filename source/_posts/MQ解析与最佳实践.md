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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UZHDGZVX%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJIMEYCIQC4faF7NLE55%2FuTd5mR%2FrVjciU7fqwP5%2BbwVhgR%2BpkuTQIhAJm8sjOi5uVft2o8v88BKfRfQOjXejKU3e7DqM0e9OYuKogECP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy2714cC6eaY%2FQqJo4q3AOCcfYpRdXV%2Fw7%2Bk%2F%2Bc0yKYfNh1Pw8O8HO7nurwvXimIN1n%2F%2BZAJa7%2BLQMExQLGDeqiry1XO14SYAoOOCtQcC8gPoVjeusKVamToGQNlHAn7W%2Fah%2BpGf14X726lME06wvMyczbppew5DbUGhEzauEgrsziiQ%2FgnBhxET6cR5ylgb99NuwzLkgOxdxCTeEBBCyFtM0lEwTxhKsaWTZD1NEOa8IMLODOMepjphxsvQYGng0DJHja08aIEVQ1SgIm5byz%2FPwWLJ5rloyy%2FKPZGfArfCuDnKy8Z32ZyEFvOx0j2c0aGxJL8%2F%2BMEAakMFUqU%2FGkgToJ7hr5eFeC%2FZQJEZXmPnmTeIY9YfUxI91FVGKU%2BvihHGASTobEaqBVycaM5Ix48ADt0WI0TeEfPeV6izocCqjytMc5gTSMbeftECLGgpRzq4mZ3V0fgFgzyZEKo41EgBOJNynsQJKlGi1ZfEUs9s0DNiEJsH5tPjoPtplSoDTJqTkvf%2FVQl5EOP6jSnYdkL2YEBqQDk8A0qRqQRyx4bjs7RrlFXw0mGc5B5mDm%2B3dm72GAAmR3zBpDS0sWZ6OkzQPBb%2FxJMFlgA%2B2YqusjfEWVThbMn8LtCgja6kwu5xfwhJ5CQvRRj3AS2qjDHxKfHBjqkAZ01VAQAZA6ozOOViBoighFBq9sn4u6K8PoGyQ53fT7ZcvdTjQ%2F1N3HgEj4JGFwYeSUpBiOjc7IJ9HbtMZb1AAnT9VzIQAF3CQGIzDDCLVFUYv8JBSCenbRH2BdIE4kFDtikeaeskL5ZxdW0eWn1pBCmO3wmAp0k6WIX7H3twPAsPtBx2DCbvOfmM9v%2BwXqDrRbTutg7mRkVtc4xwae72p5TfXHL&X-Amz-Signature=aba03f008729bfbbed3c443af7cde86611e674996073c9d203b38c2d173a0655&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

