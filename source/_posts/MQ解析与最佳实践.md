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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W32RHWJG%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T040049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJIMEYCIQDTATEOwraqGAmonyszGQsWK7OajEqZo3S43GPn65c6XQIhAPsGwkf8Pzlw0uWnTmzBav41cIf7YAu7fELsS6eGXZ59KogECOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzoGpR6xWunrTi06rgq3AOYgp7WgXVwMa1qxZ%2BdrtVCxrGLiVswzTHFxi1XsPDT5FSp74pV0HSIw%2F6GJv4eh%2B9eYIJgqfCdBsIOLXo8Krd5SvRkhO9IaxTgot%2F%2FZUywhgycQiHSJoqHMxqgMG3bchbTe8FJBJX3QcIMPSVTfsEvBjeMj6rwPoO5bt3dWJL47laPveztY%2FnZAEyCA%2B6ck1El%2BZRd%2B8JBwhuwF4h3km4HYkaj3R%2BSA8uqi9NCk5EFBCv%2FigCHh2GTEAyYfDZab%2FbVVwyUZvYzp9VwRiTuYqucs%2Fu2k7w33mRtNBfxKMNKNd4Y%2B01MAUSll7CB9RzfHcrDnG3JIx8zjbaFFbEEiJONNA1KXyhpFsZJei2D4g8S%2BKEOvf7zRDoQYNvl0eo0VknLt2sGLHJu%2B9ccfZ81ZWH1LqkmIyCntXh92okV%2Bxe5YiVghbmKAJF1qwg2LAoHTd0mA3bH1Tg9x1fneB1uN0NdtYSERrdz%2F2b7sQGR%2B4iAHLWvJmiPnteJFP31lcjrEfHd5w8tSVqi02nj3pS0PnTCqwbnMy8Zk3J6Ox2t6jgw7JvoIkFD8VRYs8gXZnd4Ea60zc%2F8PTU5VsnDTWHGCojKbG8JUBj5Pk%2BOemb0apJoj03GrQNdcl49Y2UCejDN%2BNXHBjqkAX1lXLGGPr1rVfhgjuu6ACAPKFuaFsDL6YXrekGNT5a9Lzo1P4HvMSDz4fWlw3KK4EaQ6wjLl3astfVXlnqrZcD%2Ftmh%2B1I3KHR9k0tTohE8eLmY7nRcjl4XEKWBiG9iatoY44Ov0wSIHCbNYVpvkdb33f9E8waSW2nqIIoYj97kxRYLqbalMoYn2D355WIPVvcVNCOHMo7wFLmq2W3SzNntFOOFH&X-Amz-Signature=21677dc9dffa9f6885db37d292f083735ad4c1ccf2c9c6bbdcdc0a9e9ea42a18&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

