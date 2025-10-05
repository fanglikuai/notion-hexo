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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QIGX7ALK%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T090042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD8eH8SGZiXcwy%2FM0h4wyJgSBwPmi3IqcAZUenYW3EGdgIhAJg03b1%2BEuSkJl7kpMbXyAn3FKkMGRpwnvCjkh9Io%2Fl%2BKv8DCG4QABoMNjM3NDIzMTgzODA1IgwfvJvwQ0TRT%2FHmgFQq3AOCjbCQjC8hhjCazYqL4%2B1bIM3Cua%2FIx8wBn66GXnSbNRCqizSj6KASQtfpA0UodKlDp99kMY8tHcH44rVAA6aQcG17diVRep0d6Ev5qzD5Xrs8pGyjaX5jwDT2HNVus0w7MVMMmX68n8QsfHguPFid9RWqGDp9aYqYLDyXVtdYtwXWO85RqAISK%2BVVFKZLFM1jvT5D3TukLUMBXbQOvnWnWRnHzUfaJELLtV7wAriRhXjKuPA8Uy4P7vSRigHjHL8%2BM7H%2F9%2Bdg6v3QFtXcxjb1lfG41VfXM79lE1sugGnq5Rv4FlSa8jBVziujsviIaXFyPGOVVDXURny8ENGFMcTNkoyaTKDSB6QK2nTMo3DneVKEy9gLjs0cAwBNZ6Ek16we6fZI2JpIr1lz1VyZWwodvV5ZUXorsG9X%2FtqaMyX2eHYVt6E0%2B1RZwbK9CRYwmj8JJpvnUgxp4YhqetkZgZ4CZG76IU2xh8HtsqY598mJIi6KiupoN3dtu58dvrmzogtfTxoLOgpuQBItbxNd9A0lHcV2PBVFlEDhO9cS4Du26Dnzwe9%2FcV7dDYZF0GFwjwLOdvdsr7QeJVTHxE%2FOa7eM6GffVKBtCM2dNsPJXjto0dvzRoux%2FzH4EIA2tDDg%2FIfHBjqkAa8NR7YPFxPG6tDuUhKXYhZY%2FRnd%2B%2FJ4ag2fwht2KfQzAa5Cz6QQrrzC%2FxZ0vsE4gWKw2L0homItlmI4T0a1BtoW9r0lHIH7Ic98Nuz%2B8gjrKjHAA9LGCXlkhCJ4Z1e7n8L0ecZwTW8gqGQJ3nbBa%2FQkAOwK90yfoSf21bHRv6dBbAcp3UvyCNiDY9YcUxRC5Vo5ypCENVd1rnsLso3ue%2BxtCDla&X-Amz-Signature=a55d4c46bf26e43efb5e578675f29dd4eb9a2feb55e9a8f10d90577e56dc999b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

