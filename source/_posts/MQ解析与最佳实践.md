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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666D2JGT7N%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T210048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJHMEUCIBvsszpZwN%2FUnOO7GNh6E0kUX4z0lVRcjno6pPntUHeTAiEAxerPwODFDXM%2FwJBH2lxd1rpLlSzNaSEiy7Lz0QTj70IqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGj8hE40QBlA9lEvZSrcA9iBw93jqfjRByiSopqDUX6zrr51ShscgnWRZQB94TSy4NRUcCdkATj1AQoxZAU4SKbVAw3jREYzGc4t7ObGrvQ6gtA84Do3qXwIRqRmOAx44T82mbvkBpy9%2BrCc274L2VriZ%2FKhjGCcqvPW5BrHgaQm%2FsEIThRSoQCsH4CX8hdzV41t%2FW%2BtAl7t70qMS0gMjD1X7FsiPgPlEtefePrUl5f%2FydlAabP7xiQKTvVIafA3ym%2BL1CTnFgzrKEV%2BXk%2BdDgTgsjbIHWIRcs3lCqrSekerVYYjCWhrHP8fHAoG03GRN9c4ovhn%2FBDLkxGqaEbVJtkEW074Fi%2BQgR6NAV6DjUP3LAglJV2WPgGRiGWlAz53XgGo8NOFwCkvPGxsuw%2BukS1IX6gVzcAHHdsOV3ez7dnvbIaVPJJi2GnXsHCqZaIm9NOZVAv6vf0quCGl7miZPcA6rbyrA6OYTUXtT3uu0Anrl9cllM%2FACxQAg32KxGQD%2F4Cd6j7t%2FkjOQzhGhG6KdrR6fsOB6ewE5huNrRis7kHVvznQprT7W3g1c1H1xpqsscNFzAiajxBFQHsP3jF7rYW57T0ZvgSfvTf3hKSYM%2BDaBYSxrQCOLtLiWF7k4WK2zfMqe%2FIAhkmw0TUTMIn2sMYGOqUBbUd%2Bg3OO9X6%2Fa6jEFpqMFeue6XQfOqGjmCzEFghocLWni1gvYnDAeg4%2FOPS1Hqw0A%2FcXGUAlSnIa37NNpbn56xTfqgsAECyg0Tv7USFni8L1%2FDpjzN%2FIMsJYqGc5%2BWt%2BRLJYTfubW9C1HmKv4YvYCsrV9AL1bvUzTlExYeulssRf4r9gkHmIYhPULwKMVzwL%2FOFuZERQZptgbI93jYhed3mljg%2Fk&X-Amz-Signature=54ed0e2f009b338468cf96152388527306fab6eef41b8299c395de7fedf3fa72&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

