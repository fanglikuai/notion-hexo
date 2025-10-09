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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SZM3CLCK%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T140118Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJIMEYCIQDGipmxiw7Osvs%2F2GUzmQcYllV8RhKe6wlw2PaH2CFtGgIhAJIZVDMqyEQANFjeWv1yf4w32S9ssFZEt%2FsfNEK1lrKqKogECNb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz8em0mz65Bjnr3QDcq3APYJrepqERYaVUAuFwKtkXfMHGbrJMxT6e5ADaQbdjE%2F6bijmWgnbQ1hVrVdKcL4lxr1AM4u3dlXJ3JJgCoU814pnUPX0D%2BlE8LVmvK5jaTnPEs6msdWV4PNlxXj%2FtxxmDOjcjXj8pmxQX7e2jp5CXHqTCa5PASNnJRzA9Ap9VDekYa3F8x4gdrHrUO5alhebZ%2BpD%2BiNzkahbKtk6u2Jnlc9IyD%2F%2BQIm%2BOxTMhM2lLQ5%2BTB397PVgAV4eLg3wKzzJjlJJwlIiajRpNR2Klk%2FypegVBB5n8uc1JuwjdgZZYjUqPBvVszpt6oE7Lr3oT81lUDP0dFo27Lv%2B5rCSL34ol9ouuNF7ISsbBuVOS3wmCTg80syfLQAgtHhMiywtKJhPyBckcRWKHqoAj%2BbKvEPBVw4I5dAq9qvsSGAH%2BC%2F0woG5HeCphi0pL%2F5T2xokFBzNHBTrVLnaNaWCxkTbDGTtaIffLedenrAZ3tBv0%2BfC1tmYHXHGfMeHBRJv4qwPwx%2FfrSvp1PXQNa9eyvr0tlblv5BZicv6Q7LNKixBdMRhY2hHFzUlfcUW1mTT4u%2FvD48lr48eKxpntaT8VOaXqT92x1lcuZkWjd1aVwXDCqJjk2rS9x5vJyxFBy8zDP%2BzDL6Z7HBjqkAT1mf8taAoSI5kZ%2B7CIgE9YMqUBmJ5oO8ZmNTgd5IkyC4lzvCCNg%2Ffmm%2F76hDaLMLrp3VOdlcR5PvtCF8aWO9XpElALxrSuUVrcOnkT4L%2FCHnjbLmd%2B9M2O6DNPQ%2BgNpcgEseFnM6MqRv68W8jojIBOBOcAx85R0py99yBV8%2Fd0rY3K73Q1ezg%2FcxuLKKCpY0ObZNoL%2Fy4Jo7NY8x9fxSDZjK2QR&X-Amz-Signature=83e80d1794e98c64c87f340de8f94e5ee8dc38674578d33c8c3b17e00b345e44&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

