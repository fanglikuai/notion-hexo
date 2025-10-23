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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R3DWEBJ6%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T200046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCH3YxdrFXcWrOQCtEygDPiYqPwZ38z%2FKBupVf0Qg4XXICIQCdMfq9r%2FLlSZYcz0airm2OKgw279dRzEmtBJhYlzPm6yr%2FAwhNEAAaDDYzNzQyMzE4MzgwNSIMM9E3iSJSjaaJq44aKtwDuiivSyEiwFQ2zyJzsnjJTMNSSOh4t%2F4nsyS0PotYAh4eGbvpeNFmBHJtN7kYDLOlY6CZPNm5AxMupGJAyawDmrYA9z5UD2EqUBoxeqM1%2FmrQat6gxn14BuoC%2BKvbG%2BKDFL%2BkGxkkvLRs%2BfZ%2Fxd%2BBwK%2B0YuGhtR5uesfu%2BYnoBlubMI%2BBmn3kSC2rj1YqhJgqO94jQLRuZhQjJEOkUj%2Bt4NhdXlKhFHLyrvTtubHpw0tcvvy4fXCUZhfJh35BaZ4sF1T8uj3RM6cgA5kS8zFdGKRneGNaOigyo2QzVKEO3ClD6dPpiaPkNW3o%2BszwkYu8t4BTS5KrQN9XUHIMRYuedWUje5ruVr%2FPEA2CsHW4pOmvMriNNAeU6LPyZugE7ABSE9XOpV2H0a4O0JTssZ%2BqShL9947N2RNn5uXbniC84WM5OLjC3QJ6BWMYrb54wVhQEe0jLmGKbgz510c%2B8cJFG8zK9aJ5qy1GNcgblHjjrwu%2B1TaLFEHKoaCLELJAgZBGLRwEXgjRgCdb0lNz%2Fl961ukXJhbPrfjXeK1si7jfXP8zfEHA8l0DWUrP41GsW08gSL13INjHgcDkNe5LSdygffHBOV5fuT1qccdH9szFIOy%2F6WHERUHSFCriDjcw2oTqxwY6pgErWsWYIJ4q9e6pA%2Bdnhv3Bsy8ZfxI8nKCkYYYnwevfnCd9SS2CbaGNS8mh1O%2Fi%2BTkuGUa6ycr0OQHMq9Ue3ePeyQm%2FkF8vJJHlxCdMsmjRq%2BfjZGBtqXTebKF1q6MxrP4fEIoVyyOCHrOn3BIb6fqzciv23Oi1DNvBYwV2xhxu8wlKeJ49BEn6gdvMpZ1JsFoo7giNFB3BuY%2FfCx1hvlW5B4%2FsiLj%2F&X-Amz-Signature=09c0b03f775b5c5fe66b93b6ad1f53a77d9820a4b4f626f43f94079aa77bff64&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

