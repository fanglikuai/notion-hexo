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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QIJYBHAG%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T080045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCWd8TAPGxKKDMzyjBImeTjCP0RiZnpqykdL9DoE6jg%2BQIhAPj7d77r246WOP%2FEKoT%2Brvidop1tzrwhmQKOb7pA%2FFtqKogECK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz6DUIz5%2FNLW0wCDTEq3AOIoI%2FWWiyKXyfeeLcBoI4IueCFRs2%2FHvAAsYT3dnvUNRNGxee9kFZA18Ua49gtM4CTbYFItMFsHLCZM5eJNmuyeykLhI7BFHSCOqrfjDoAupZQTJ5XqBGvuwCx9h7U%2FaiMQAz1JuIqJglORlscIuA7IS8SlQznqCQS2LbvngmdPGB%2F5GvqueqZmvbDm%2BFpdPgd3VbtX1zQJ10A9aCYZK5SJ5WUNJjOnmvtYpSejbOpTL2BAlinBIr%2FZO7h%2Bw%2B1nvd5k6YIue8GN0V4S3i8tHdobm8jirgG8%2BmQxuHbcYK7j0r65NyFWqhAj7Lj0EyPt1eqEXhf3qZEpCrjfVvZgioFVlWUtt3%2BqD%2F1U84xbXi%2FsWrdBqe2UQwU9b2lR7V3GIxLl8HZLn8XlFiGgdsQiPQHTxl9zEtDwBN496WrrNX%2FrEp4J1nF3Yo8szyEflrgZy21AVEGNwKNM8sx449d%2BVPh1eWFqvqb3d2%2BLoBG0HqStkD3JOCEkC%2BnbulptmYeY6QNgAhYp862IZbQ09nZKbmoaBum6SJUr0CaA3nt5Bl6TG9vSyqSj9oIyXHqpPLrz7F%2FfZZOTPnXTbmJSOXY1TjukcHuC92gWYbKjnJgPiwBpwe2uIIm%2FCwBNpjbnDC7u6TJBjqkAQeCQCG4vGRcDc45J2vuUhNws9jFX4V9lYiEZ7xpHzSA%2F23X8vOM52yruFbeyrdN9GafQ2gLoE6Pf%2BHnjVylB8LPYrsZwgeyrt3HGXH3QRKPJIPXH5YE%2BsQHkw3XyjKXWRRnce%2BLYmaYHiQD%2BGDib5CJdGTA9Q0rcnbqBWT3M1SqRcuW6hL4uwrY4xeHiDSjTfAmChWFRBGVXWlwJkunji4mf%2BLU&X-Amz-Signature=d508ee4234b7eb1524d9228be48291ebe4a4e29ff9cae462dab2bee40335109f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

