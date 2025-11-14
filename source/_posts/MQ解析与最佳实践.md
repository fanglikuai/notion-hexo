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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RWZF4JZG%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T040049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIF7HaATBXnAhNYhWMiDQM%2BFB%2FFm6vfyr0AlrfUeQLhheAiBIThsv3EVkFRMniwa7oYmkXOe7BNwl9UPl%2BKrARTModSr%2FAwhcEAAaDDYzNzQyMzE4MzgwNSIMn%2BUrRB00d2lLFRkXKtwDoURwnKaUZ0JTj4SG%2B4eHPRhO9j6qBTJvBlQAKRlK3P%2FR4BLDeZDxX9nRta6DTJI1Ab5PudVBBnbyky5P9sb5dkdsjtgenBSpp9OXJ8%2FVAKscfaERqzQS6L1ViFXNtbDi53SBEsPHcpKsRLdCdLv3Euxtia5K3x7xckNG3KvDaGJB%2F1x970N9gaFYH7bWtbH4HXYE1gGEE88yC%2FYr0sLjDijtFy%2BmURxgbjIt%2BaaOMGkO2lE97Xy0%2BsnyWV3GpAVmFN5sZ2ZhPR79z7FQYlJAju3BxDbLCRpUh0xbHSzrpzxZ9Lfv7SWIdoiHNDN34NRfrUUwYVYN1sOo8%2F75mgV5jqJUfOollWm%2BxaahiPfs4n5d5tnFAU7Q4U8Yfiu5eZD%2FsnF7aXxxWvWrZrr1ZSojwygsPhV1m5PZpZswXKMQlJVUJ3nNcukcPSwsHDkc6N5dBwFTTiuwtyoki02EaoifhoCD09KQ%2F6Ef%2Bc%2FhffZ51F6TN%2F8gyUVayJgfAV%2FPG9XM5%2B%2FZPs7ff0gw1gTINJRgz1gKdfZs8JouAOXGUEylVPgrlqpq86py%2FU73W3ZSArWtEnrimR9Kj2eShmJ80FOZWQoLefLElUoeIWR6DO1wvfoX93wR7pFB%2Fa8YlJkwtLrayAY6pgHAKJ36P7k%2BFKK8eXiL5zUodgLwwEOJsmgAPvlUAOcbIGfTMJ1ChHDHRPY5VGZ6P7LRO0MJvOcY%2F%2BC7vh3K3zk1L4odcmGszLOHD4TJurUr%2BgK80PfaP0GP2R6HFc71dbulFvYN2JyBi2kdN%2BLXqshvPXhfO94%2F7%2FbCwWyteEcpk1IVZ9rWWFmhvGvvbaL8M5kodRzkBuUA40x%2BL999u%2FYkLzzp%2B4nk&X-Amz-Signature=277148e1966190cde63fd7a203a61da2b2d5b6d026d088bd1c660c1072c8094a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

