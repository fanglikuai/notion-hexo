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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SGMEXELE%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDNmcqWwnNYbPLeea%2BvpuaOWzQON2kBrlchvimgmN4WqwIhAJp5s7UTUhGl7mkc6MawemEYEY6iu3D%2BVrFTmYLEi5MvKogECMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igzgtc%2BXhm0%2Ba%2F%2FJOQkq3APp%2FZRuI6BdSMkmJbjSleblPMzWyFHty0GPbRx7TnjAgc2sK1Xl9a2bAEjyqkdstvDW5nyzZBkaUpFmgc%2FcVwXrKoEOdFZAEOMnE1eif1ug1uGYC9OnfZ0116hAATY6Rr9gjFqpyR5bHqqnmApXan8%2F4sSfsvyRUquhiQe2oJUp0Or2ZqOeutsJ69iMll36kq1tA5LfCRGj5TW7h6og6XMRpuB1QjxtRuZC5ie%2F2GyAQzH5PIf0lgKI2hZ5y93EsNx2mw%2BKDmSoTErxgf75WMspAU8%2BfiKsn2O%2BUCp84HZtwvjmTERPSlh7q%2FoY8uJzrVHfug5RKnYiDxyWC6QZpTIk3wLz%2Bywow99%2BHEc%2FW%2BAC0snCuMfcElRcDtZKN%2FMCNtqo1CSltVQKSvz0OcuCZF2EyXnzUgpOeNwUX7AwmS5u3FHdMvV8v79ZRI%2Bnq5%2FGhWYUveHRetXrdh1zYjjHCT6729AZySxQuk1oXgGyinmnxDOhwCk1WBaop0hHX4T3onx0pcn0YJlZrYo6jCcB3KauPnGLCMrL6oCpM2pCXjvzdYURNjISnZukh5v2dgNG%2BVy8J%2F2F7Z28Ifj8L0CDdwd8GuPt916tF%2BVwNAVJXl%2FIvUxHzpzEn6nYS4wjZzCSnbnIBjqkAagBT%2FjYiG%2F5dTaSrstjNXRzVr58hDthDRxokW5GjPu5zyFDWUysuwbZO2QRgqh3LUz78jFwXveSMn0Zx23T%2BdF1W5P5t4%2Fh084QA8WCsNcH7vCmfYHWZb8eoio6y%2FCG5p8cgMtXIUPjeJSN1gWURoQw93pTmYDJBgAVQrecAredRhi%2F7MVpKbN%2F9g%2BrlAHrTBX8tE6jCrQKrQTDtrXa6uecrqoJ&X-Amz-Signature=ca88bc8c41eb6b129e4d93d81f643bddaecbb650d0b472b07d2af07c70b6bade&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

