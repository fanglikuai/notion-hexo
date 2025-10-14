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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U3ZKL7US%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICu9yVcnD7kQMwAWrXoZDPd6aFZykNzjCyy4MtvEv70PAiA%2B7PulOPM2tSj7sEG7blkpMS5EIoRSEE42FaSKVSzsDyr%2FAwhTEAAaDDYzNzQyMzE4MzgwNSIMD0xwF%2BxxW1b2veqAKtwDu6T8DcmArpVhrIbHkHPR%2BzxG0Jg%2FM8em%2BcnMsZLmTk8M6WUDduGvEkFmg1yBe6azmwzI1GSPMpZXskii5USUL2B2SdRwFiNjrQ9PqyYup5OFgBpJScXqU4Wp2xRcVk8vVOvsaP5g2eW2MN3vmTwtnq%2B%2FhdISOK3GJTfTwjkRQzHpLempy9zXa34jxgYlcSARCdgl4sWdlJjRi7jl9ua2BnBk41nZKyXyk4KOKMmZrMB0CSahY6DUgijkEoBL1Hm9fYkiLktY5Bp2LcyrcWqTlMpyBQW5hzaqFcdu5JpJ4qmhUK8VzSb8l5jNZPyQzKHQlPqYA4vJK40mS1iAEeX8Cc1XO9Rtw%2BwkSNuf1aS7DvtuUrh8NilECgOn1CO68gz4dVUhcfxfDNONVLHBG9ZUXKQTV27zLqos2SZL%2FQvnZrjgfQu9%2FYeXJ5H5nkSuuwZv%2F7n8Egd9x1ArBNkjnNV1u3%2F5rTmb260z427TOwjWYvx%2F%2BW%2BL65rzhMdlVd5W4482ttzpIdqDNBgitQolQhzbpT8RZdA2LXzVQs7GDsmTzclcPnq7xE%2F%2BtQDgPYs%2BQwsBU5YWMOT9T9a9FU09cldtBfq0EFCc4P%2BtxcCamWTsYNScWfjs9DKGC0iIa1Ewm962xwY6pgEGJGAJf5U4dxGaaducOJICH1gNsuU6tM%2BE61B542q5Yl8ocw8%2FTQjw3D5QX73fBguFPZfkjY4DJGM2HMyGQrZ8%2B%2BNi3iafYT6fo6hHntAifjEE8AXvpoJ7dyhNd9FyeYzbj%2FBj3MPFXYhHHvCxTYDIYiBIAbD0mT%2ByEs9R1qxS5Jg5CQ5EFJ7wbrAdeoPqXybO3vKWhwIdTOyNG51%2FepkJrYRztPwQ&X-Amz-Signature=b4a2a7699b077673ed755bafc3e38b52c0770c56833ebd10c6bfe1c2268db5dc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

