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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662U2CP6DS%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T090050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEUcRib5yN62JiHvtLZc21q4gAAgAVXpafUh2bncCuMeAiB3JYXJ5hZdsLap8zWTZFRB%2FJGp%2Fbfs6HU%2BwzdDg2%2Fpbir%2FAwhxEAAaDDYzNzQyMzE4MzgwNSIMKCF5NwxauAUDNjLPKtwDm3gDQAqXvjjYBXWNJ6tpAej6P6r5KOeWt7L1rWMrmggs6T6aTXgTMAIK98oYQYIMdwbILS9w8G1sQlP3OBFsUPCzMzQiwmYDU3N2NcpN38IDQ3M7bWIscbQtfhw0VSUoxXAnMvo1wnMl%2BSgGJ%2BpkVoZ7ApL8IqbNYb0wO3YFUBb%2FkBGzqcoPihV0HjRZoMkbr9SFpFBVlpY3I6XGM%2BsnAN3eotVeJBzTD2vfb357575vTkm%2FrRLp4vFrcHxWJFLwgBl0wH1WqSpdub2VdQdjl%2BbrCWbL5iJIlBBMQMj8wtBAmklDaN3I9DnhMYPG%2BoLHS6GBY6ISF8nRbKJ2BJ99ElGP5fQ3gn8kJPyirebR3uzYKvejvVs88mm3wtSeh6YD6uFD7Oc5XUYEAAookVm6j0pNJVl8iK2HF5n0fyZKzK3mwHCFHELClLufw1wNn%2Ff4lRfYvyBR7fDacuer8Pjw1S8KcdRF9mt3HhUFz89QT9pmvRfMPpKM4n3xNc1wB3Z9zfXbGyMaRdwKlsBQvgqGJiiF8DsngDVRo61BwZAR7dK7Ll9ToswFsW067pf2C9x2DMbFW204QPNav%2Ba2JL%2BYqE16ZJgyMyhcca4kXI9J2qREgp7bQ7e5Od86M4MwnKi9xwY6pgGEh4dw36%2BHF%2B6jAXrIA%2FSvMSmJVNuJfy9xEQrhriGgdQ5Eqk%2FP%2F3Vy1eK%2FfZ9%2Fe97XXDdELI31MPJQCsT%2FYGz4S9guP4F%2FmukEdPkJwTjAKPY1pvluc%2Fq3e8Ae5Uib8BjaMXYzph8SUI3B4XdMdGjg3CUNtnMDxXtjX8dKqwqwIOfOLY%2BIzxZkvtk8TWwvlPb94H5Faiml6YD7CDmbz8MsKKoYq0ST&X-Amz-Signature=0d9b0c6e409ffa1a7299e7dbc2008600419a888896fccbb5bd8d97195633405e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

