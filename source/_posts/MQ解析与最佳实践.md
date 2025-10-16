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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XCLKWJWB%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T010054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDj7mFiNCdL1yEoSikNhLhF2G0YmOuEDcCDxoluihWQbAiBKCIjY25oZUdhOoA8E8Ecq%2FprykYh6v1gmmfUzcPwNFCqIBAiC%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMjJGlAb%2BDcFEzEtarKtwDNwIsCO%2FMP5T9jcMQBbCINPTMpcnzLPybTV21C3i7wN7QtLLlX%2Bwf%2FhbAQGEBQT%2FGCFp%2Fe1co3IrGRiEK%2F3Kt2hLNLgNylHY5CdGUgh%2BlLQzi8vMQJT6yij47enYWETYgg1%2F3n5bLhJdZGIaD9%2FEn9qb5JuT%2FwpkrXFprfGk1VMmEIAfXrOwfgFzEZQWT60s2BJeKTw2MAO480nmojxKBiIcuH%2Bucn8XcRTsSjep83PyT7%2BzMsTZNVfBDmY%2FiM6%2BTsgZoMdAy%2BuABdBYahHCOQmw9y%2BiHuQa3ckUHmDDgwusqKeAVUhaeF0Lts1mYz8mfHtfMGACrRVM3i3uhFIECu4yQjSdh7%2F2qAf3l7gx3M2770KgxJPLZpW48yAp7CMWi%2FxLEhJyXJ7OGCiI9Fq4m0%2BumofWQqF9XBXa8TopOfX8Au8i3%2F7xfQWyrV7gYRJ2ZKoRRjKLCjlpIwA1oMA3e5%2BW877FSGsDuBh%2FIHWbHZaywKpRxJyWYSQBGAbHOsLRo7patT0SnqcgzemuMOBjOj5lbKjtanQmWvZuhDyddJWLgCGcQl4IKND0xYgOotm5x4GJu06WlrtsXLXjeKeVwyNemcWyxYxX3io%2F0uqj1GT6PIb5qefVp9VECr5kwwvrAxwY6pgHOE7czl7wIUbsX1CAHxJP2Nh44KQXD9loeHd%2FJFiEtNZ5zbSLxq1iHstxV0Ejsd%2FVgAKsYTlNPRf8B%2Fk%2FV2eDQhJINMkoo1Le8FP%2BOC%2FbPpNmJV7MUg2SddzKzlW7MEwfYAbZbvZ0SVgjS7DWir80fr%2FMmv1QCE6zujQX%2FiWbcv47LBlRsB9gLDN15Mu881xrcc8XSyEZZ3mj8s0WOIJGpqiu6UZ5J&X-Amz-Signature=bd165bbc732d668caa05194eb84989e06042b6297418996f37a47692dcf3f523&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

