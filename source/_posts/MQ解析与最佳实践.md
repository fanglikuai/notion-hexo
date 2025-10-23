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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RZ2CUTRJ%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T130102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD1f7E31%2F7pweX222zcFS%2BwcO0XUqV67GfLXZqUfEvWKwIhALFbiz2k4da6Z9ovxVPmOM7IUY%2BaChPbxYbyJv2Rewk9Kv8DCEUQABoMNjM3NDIzMTgzODA1IgwpFYSXhgQW1Sqel8cq3AMBEUwb6VKFMhJydfXLM6tkkp2iDXnYsp1TqXYRVvejxvYQHt%2BC0FrCIpCYD%2FgjgpaRZjtrut73dJXaNcBaq37h9g63TKvTmDmo3QeRiQc2ruEB5b0S7HUarz2G9wlLLq3Z3nn6HFHTsLjnuR%2BzUUc0w32B%2B3xN8nJI9pkWAOU5F35fpFt5luY9yymTcv0GIUnAuo01LZa683jNQHt7wD81ysMGPm1MuJGwBKIZqenkKCim2%2Fjl0dOZYVTJH%2FhA697rEfzbUS27KYOLJTHLbk4TeYa0jmxkuA4Yq%2B2mLxFFjsyShQ5x%2FpFJ3AZzojIleqCH8eIh04ATNYGfDmoMRgQ1%2BJ3U61EDHvOwYs61a2zS1Hybtca6ZpVTWTyVfZaQ7Sr%2FlheQOAfCS0dkoytpvrO7TspDxIsWsJmNlX1y3Wk0I1UXKl8bnndZKo3e9Z%2BlL5IDOv7klGTzaoNXWIhd8ZLmLzhv3XwvATiEPAPwx46Y4d1OQvVhsiIlbSSyl8uz3eyKRBHE2c0VBa70NkEWs1t0mrqZ5V6uoJCyh3KnsH0ocVyuX6hzH7F6llFdDbDBfF59%2B3Z0XBR%2Fc8EQG03ac5%2BSlvCvh%2Bjx%2Ba2zMp1mKiaWi3%2FsgMmGZsKUE1XsbzDStujHBjqkAd4jEaGPynx4g1lcl%2Bg02GjngMdwzjXTLT7KEHXC7D%2FvCrMZzdSQBe9uZiA8Fa3oss3m5F5Wnrncki0oq4ZUFsPSRNd%2B0eS5qIqHXxhSC71HS6fQWv8P3dg7suZr5krZdmo24G3AZR%2FgHL21RELAdELuueYBuDGznfxKFKvzg7y0z9v2ax7e2o9hFp%2BrX16d8nDfdBkesz7dhfTF1cDJPlXo3rCV&X-Amz-Signature=d9ba922ede9421a1bc2e198d89ee3a4da87ce9751e429660b9d97d5807996aeb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

