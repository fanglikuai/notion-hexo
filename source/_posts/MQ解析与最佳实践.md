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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46624JQ6V3Q%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBwaCXVzLXdlc3QtMiJIMEYCIQCu%2FfQXvtkmZSpVYNVYYieACJiux1AGM%2BQkwMIS5ujOBwIhALPvP3I020sEUXxgy25L77PJDKsiXt9UqYI%2B3C9a0iKKKogECOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxujie8dxHRIKYFuPYq3APIn8Qm%2FVyX4sWqSmAxu49QP%2F8bUWcWcegsaouFzGOMXLRKksof3YDB0gbegDNRJkmYlIi54Mj7hrIG09dFziQk%2B7IE%2B1VXNf01IXF1sWlfaLjnhxUpi4iqepsSwsBkNacJP0%2F81ECOicxaOp%2Fkn0EU%2B4Fed8lhkj%2FgoLMLsED3JoF6n5HEJ2mGEdjqPYRbIlZP2%2FlHAvMTAJocHXExh1gEn02TUCUt21%2FrliP7wOYWuFBqvGu4cWVa0Xdx6TKM77PlhNlbpdyVKcHK7vjzzy0Mb1%2FOXynW1XTa9NJtk%2FoTKbjTVeIhJD6LxdBNE%2FXj4uDbAvQPVij%2FCIb4W5RXfo2rQqblKUUrrGiNKYo6q%2FDQRTFNw7VYtUU7Ys79qBKdr9t9EFkFIUU0I%2B7siwOtc8bwt1H7vuSLp8TYec%2F0vNxxG5IhrW%2BnVnBZr37cgVcLNRK7e9gISJfiuaGEONZ6pfY12UclzJVIPTOX%2FFVfQRdLhuDL4ENrpimFSW2oFsD8vlHR3N480zA5dMtfvquRy6tXqmCvIBzWZ974io7YcI6Raq2Ojzq5X1zp86wj4ONNDtV2A6se7UU0PCWmbnlHKRQtRgNWw6%2Bso%2F4vfN6U8uAnJpv%2B1u4hkmRnQ3FURDCJuvjIBjqkAY%2B9fY%2Fca4k0LPlCNOCzAMY3qkQgT91xOpzG5xkcnysitMCpcj3%2BbmwTDplc4yi06AqUoqtD4Rea2j36ljPGffOt6RnZr46Mn%2BUZzkSscIq3igQ%2BZeSisvovgxHY8m0tLqgC%2BaiuCxUhh6kPThJtIlwkH210QuGPfyKg8V%2FWILZo4bYChr8US4YknCxTfX1%2Be5kOYRtdJ2vJE4n1jR8Qv5MzlrGj&X-Amz-Signature=b62887548cb0209a8124d9bdc04ec3015312382f6149f4b419d0ce5796e4fa68&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

