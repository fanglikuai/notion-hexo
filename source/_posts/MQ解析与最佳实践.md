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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666FZ7RX6C%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T050042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDQaCXVzLXdlc3QtMiJGMEQCIAMfjh6ohDTOk36pV03nRlX%2FVebLuvYYsyDMGDS7vNgcAiBR5XREhrljwOatsA0uBciucJGLYmSIS5X31epdoaHC8yqIBAj9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM3p%2Fk8au7bhh886UxKtwDH7tkjb%2BgXNBc4JDHHSdYMYtUvjnv2Y48P1OZrHgV9z32BieFZYvZQSQxwK%2F8QaxMO4nRMAH02K6qgpwIbS%2FI5kS93THlUZu28G%2F5BVgeIfHA84EI2fxA1QnQNjdDpF86H1y4ozEAD1D%2FE9F8xEcehPh7CZLKFysAVtPJPn9GhjNKKPI42nlw2dQ3FNbboCW0RhLdZfgwXvWaf4Aao773p%2FZtX6vCumKbnqNU4JHbyv4WzI76gUs7bf8gDE%2FtPBYN3LPA%2BbBZHZNdm7Pph5qJdgvzsZ75G9HiBSicX4mRc7ydc8S8VLCGMDC8zOhEFf3xsV2ZGG1zevI3tq2GIaqN%2BY09%2FgRz3AmPSdOnO1T6jWi3Xst2jXyfxwXhGEG%2FBcXDFhm%2Fm9L39EATQQn0%2BlKcYF%2BGZWZq6hltrQwvcfypjXrw0YiLqQP5mG%2FGWvZ2sMs7PQ31gWPAL4gRrUJUSNYXyd1g2EwIf1FgK3mGsweu85%2FJbilIA2yOTksnXtl7qUjyFhnHUikjEcc3U9LhQGtPgxCr11FOxD1xg5G2aHOQQPhgkdcY7dST90UZmYEIPMZm7YPflitb1GA1pqjC0dZ5walXi3Or88RKMZt%2B%2F3hqlD6SnjAeUpEp0NZXvpswprnFyAY6pgGh4%2FBBDSo1aUT75p5y8GLxsUaNvDWLf9IjyTw5cBpo8uvR5YJ02kLW9x8xtT%2FM6PWOhVTgKHtoynnJrgscXYk0DDKQRTqmC9ulAj%2F6EZwoCfwnyb1YL8%2BF2NAQ7LuiyuEE8zI6nt3CDj0Vp2jWRZ1x%2FUVsXFIy0fGjzm1qbC6Vwjru6Vl2azH%2BSZ49H4Zk7gaj%2BlZCB1hQx7%2FCzRaBOPJuaQnPFu5R&X-Amz-Signature=872042f52f561c56a0a4942509d9ceeb86ed29fb86e845f2cc580722f1733ee5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

