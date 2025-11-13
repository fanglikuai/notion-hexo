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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UQAZ5URZ%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T050048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH0aCXVzLXdlc3QtMiJGMEQCIB8HIQc7nB0sKcOvyx7hzeBWpxF49LUQJ69Jd3lSsyKAAiA5ePVgTOaWYNRuGxMXC0xU1Izv3qPv1z%2BIAc6T8FxQWyr%2FAwhFEAAaDDYzNzQyMzE4MzgwNSIMmzOr2OboNGNI%2FLj1KtwDoqT6qKMga4qa3cn47s2OIauYGenmrOohMufHKpPhljCFqefB2wTHlF9pYTeVainsTpYDjC6igtxBMPoEgMrDYtcnyY5thDeZKL8akEO1MxMi9rSXXcxGh3mcIwNHOTtctINYgpx7uD%2BPGSfxVTat6MXROI29BH8WPUgVfOy8L72TRg%2BCgbUwbtF0SkvpwyWZSdLvPdx2Rq5qvBqdAsYzoNMRE1FICWgYigUk6P4JJrmoUiCc3l3ABR42S4Bm3Q0fs7Ud2AOWfCEzGvF0jMcvFuConeGCXz6wBwRgoh9CBBgi1VcTnE6x4OydqPMIWyyGWNACF4TAdjU0KcLEIw%2FJ9YJ0GDlAPUn%2BB2kndbk9pNuAkD41xGKQUkpfUJuKaS1LcY5a35usMrL9wULDVOLgQEkfkhtbPN8U%2Fo%2BFA7OfzyiD4%2BHK8sgWr%2BDn7wA45%2F4BpY3o66mEulwoJMYlz%2BZDssGyVgkKO1x2IJtR7rSxrZwZfbS6VYrVpsiwzoWFmzy4wQiMUnYw1EULFIvDS8tpw%2B5vrBdvBfB9b3RO7gCIZ48shahroe0y6ztfwczNyIbICRfGAjB3qHAbm4uPDOYEp4WxPnKqwgR7IWVYeaBaWxUK7t8JQuznwSNpklIwvLjVyAY6pgGxOg4dMcLwalVHOEoR1xwcsJtHZdrK1tyHzk6VSNTSXUdpaIhET4jem2WT%2BFCovdi4%2FJ%2BsbC2p6TT%2BQYY03qmBbXgKq6b8W0652hlkhQg5WK0nUiYg9BFPoRSmmWJENGjbRJldX6lshzhYmyJLzeEG1wUQvIiHdzAeoUvHIvlcYZ0OiznxNjSp%2BtwUt7zS61%2BFYfy8vIydtffr88PipnRUG03ZASmN&X-Amz-Signature=9a057447a5610c8433da953bc5e72ec1e8743aefbb7a42aebf11a6798eaaf5f6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

