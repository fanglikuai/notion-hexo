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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663YPGFG2G%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T130047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQCdWHjDKjXifgtt8AekZeyaQWDjKvW2TPN2LcqH12rDQQIhAI2wSTjWOgpvmFuJvI2OCmGXpeHxHMwNJgeA1elmjttgKv8DCCQQABoMNjM3NDIzMTgzODA1IgxUln808d%2B6eQXOrvUq3APbYOmgbDRTo88fV2u8qcG9JbPTmv4VTtdkaHoiP6t8b4genx%2BnZdjkzJj3D51vctmJB7%2BfVKSqaRrQMK5qSIUacGlLhb4%2BoQFV2Z9EuGDA1YlVeQZBAebggDJHNbAbIpby2kmCtqWCfM6KS7tlPgTJGCliLz%2BEcLgbiRTARQjpIO2wpNqz%2B7XaZESTCpUCN%2B18zWropFaTDa5MRfhjlovj6MFbVZDpIlOsFfgZtUkdi6OMr%2FPUZz1h9Ctdvrhqlcm6RwmBYe5U7%2FTFoaP1lBfFPaBAKHbFCjCtVn5KlutOxu0X1xvCXIt%2B8fbn9%2BshyF0TRJuQ1J6%2Fhlybvl64XUUWqeWGIlVfLvUmc7T9p9DJ9hD5JpUBEU7ca%2FMaXJSmYueQ%2BgehGY3YQcJ851xGHRYsohMEtgAgLPxI8Y97lfM6vmSoi%2BwDTRHOTTUwWhvVmj%2BrfHOdUAKtM0lUiuQqs%2BxKkgiUSE7tIPYj6QRMOrjvDEhBpwUaEnJJhSosDGcb%2BMFU17R8FDV2kKFPXIOtZSXxDxzN8w%2Bb8VbTk53%2Fjl57dBG5oMR6bQN9037xttVjD%2BiZtrK9VwriiQ1mRnlfKS%2Bmn2rWXjF8JgjxcCiDNxif6rd0oRFRfdRB62cAwzC2o4bJBjqkAYreBpDWl7OYdkldiRSBbvv2X5QJaP7l7m7lYfIyDzbWB6UrXxi8sl0Z3c1TqN4%2Bhij1a%2FPuipFlyMktNUt7RYFsxNRpLaUml7TkdDejsdV6tNn1keKK2X6%2FaSRG5ccormXBIbhk2Q45YCj%2BxVVQHzWmmvaDuLYGpeabLa7DaiMlRXxUX8HeZE%2FqawBFc%2BmxDd%2F4fdHJiVQkU8XTbANqk0i9g8lU&X-Amz-Signature=9639cb80e48597d375c184ad2c2f86606bdc521d028ef0eee5bfc677f34b19b5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

