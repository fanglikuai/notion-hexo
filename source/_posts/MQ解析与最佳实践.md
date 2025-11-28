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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R4Y7JB67%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T140041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDHh38yCvbAv6%2B9TRVj7YMrxIJMvWfHHGQXhtLZ5El8VwIhAO8uLTb1g35Ei34r1flZTqA6CJ7pbBzN50N64c821a7%2BKogECLb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyGrTokqQMSl%2FkL3lsq3AOfwbMFMc3XQt2uX9CfA4Q8TY1lTmW1odeY8orjLO%2FCyO4LUMrkGvlJM%2Bhfyi5vpnRO2Hpj2zvz8E9oliOPKG0GlQxRSk4SiYwuZJDVTgECgqHH88KI48Va4JJ869REuF8xXXzEDhQvdhgqqW9oqxGjO0WVEL5xM5BlSm7OYUP7J9vSV4nG7ucJPFQSmDCBowBCi5jRd1v1LFx4kTjlpMejGoTNPg34vYZcPcRiqapF2%2FkMnvLEwN0%2FFSB0PzaUi6XT4Uudh%2FwOMrVxS32WM5avfisg%2B4L4r31gIvXTiEsHH0g9P9W1%2FKHjoaUHngMuAgqZhHdMUqQiaXbKBW9xglaTfvB2ErReHS9vXP%2BzKeuI63XPUw7GKDuIpAx9OWNuKoEmrhqOi77tzjgOt58JYhvt8b38eCFzHgmvPmJE3p5RcWfRF65H5DxlUzvipfRvj2%2FbGlL4k6Js0amJPtJPs0iDURWoeCXTWEFD4ivEtvPOjJXD3TLK6UOS87YiVmuaNEloD5EQi04VRo55wjzMfDVecs2NLsee%2BCUoZY4kYQWyea0yEnm8J8aoJ75TG6hebQvu7URy8SCLl51bnaxV7zEMKLxv%2Be94eR2PlMcA8GO5HrXIH7qKO793NejSBDCPwKbJBjqkAQ3kUd6w1piW%2FGCJdUHxozj6m8hABDQz0NJ%2FU1EkI25KEvl56ogWK6mdi1Yo4avsN53gR4Owxl4g2zkcZnc0ZdcAK5dlx8yyxnsJLbVIVbx0dEi%2BVj5Sqj8zjqPMw8zjSrzQWPtAoa7hQB%2Fx6szSJU9cRVWjt3XFWGXtxBy%2FToxvTFfGlYm1ry22kH73GmcPHgG186hTu%2FTmWGykdGB2jaNQFapn&X-Amz-Signature=f3afd9a766058f658cb838e5cbdaedf06b9c5b8f79ce4752fe4b1e9c0a624c8a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

