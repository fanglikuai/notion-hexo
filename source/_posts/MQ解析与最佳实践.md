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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z4F2WGDS%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T220037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIQDyeawybrtxFv9Xrrga1yUc6yZbF7TTmqf6ITFQO35IIwIgYKqhEwtETAsAgTjvaGEPPOWaZ4OwPsut%2BCjvVLOVc10q%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDMBoGnA7e9La0MiR3ircAzi5VvL%2FShtM3j%2BOXg7VM%2Fr32WDXy0LXFkjIE3WWg0KrleqJ0cZ23YIyFjTIhomn3ZtMDS5eZv%2FvmKC9l0734Ynx%2FOBWmbp5pDYuLxFk0idbIiRYlK8QZMJjUf87XQHsYJEF24YntWtetlW8N1J16QVkfHaRUNpIOXP9YWxQgJgwXf24UVtWhaU9qvMFS2Fc53y9O0JmL6lyzKrD1x8fsRxRzLUJgxqd8zTdv9HPiplGmUkv3rfr3g7XtOsK8Dm%2BjHtApQLgHVw8vrySL8cka1QCeUv5SJfM%2Bmn2bODJT2CS7EgAFJuj%2FOm5sj6OSMxHQOcxuEFhnlKErLCuz3dbVHgCn1b1ujoZrnnqhIFN%2FIlFFjmKC2RAZmyOJuCbEVAva4Jsx9v0NcBpoaLw%2BPzKGUvO%2BRTY7mW2CoU2jfZMUTfMVTTfa%2F52vWm9IlGrLPegUt4Nupzn2%2BoG2WaYE9QvmVdjy0X5vvujXiQqAAE0he3A%2BXznyi94HGH7UAZigN0kYVk%2Bpt0s30XdJtnMtOeVYLdkeZvIBgdpmVE1vr2jpmZUY0dm%2FBUEi3181ZxnkMzdCvdQI5EUa%2BpiJ72X6GtLen8zLXSf8T79U1xymi3VlcyCBVkLhQHxWKepng3lMMamq8cGOqUBY00%2FDUSuLp9NYaw4Sms2Zt3Nqf41uazEenOmNkHg4s1NTFaUW1lBDTeV1PQrObWMjInED0g1JONQR9v%2FoEQeM7DvFexdB9GvbE43OVBo1p5Vc34i%2F2k1aOPTY3x4z0VY7UtDzwCPxyuTw9hw5RM5V6yOapK5ITz9UjKvscnevwYDWSQjvvdIFkBe%2BArWl3Uveiy%2ButYS998umjcwGr9hrsc6zEmZ&X-Amz-Signature=6b60fe208c2de839fc62d424acabf0bd2562e2897da3bcc1d48899427b497973&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

