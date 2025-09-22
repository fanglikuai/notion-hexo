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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466URDY7CS5%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T150054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFeffvxpK2QKNFZtiR%2B2mgeV6hP3zHUg8uoi%2B1bSFBnIAiA4tMange1vnqkIpzG9jrTh2t%2Ffh%2F2gU9ljii1KRWi%2FKCr%2FAwgwEAAaDDYzNzQyMzE4MzgwNSIMN6AIR2ZNDdNN%2FO1jKtwDVOOiM5h540IAoAkYN1vCC6uueU8ENAAkW%2FnEBK0KmcFbnCSNHVelxhEzCK8IdzZukf3HPQykHx6jWNa8PJj1ywD5%2FvNXHbL1fOJLjIpBnOEO6fdBohX%2BNBLOIiler0K7wd%2Fwv4aGoWFb9D7X9uqtpQr3NoHpr00GGCpo%2FNKOKupVm3%2BvGGCcz4iki018VJMrL97ZtxHR9z73cF9YF3IO5RC6fJypguGqNxgeqHRzge6BClRf5vrTmePgAJHCc1ZsrH6wQ3TBrZo8rN2NPVc92ZJDr1SLPEXkExt%2B7ucLkEC4yBgV2CG2jN5W3dMgV1%2Be6I7h2UoF%2BBJG7FXGJXNWGAxfVBf%2B%2FwzfX5nn%2B6eaHtFBjF83Va3SFjZp2G0lB7peabtnG00mMBWvMyCpsFaGDId7QMXPKOH1vK4wDuG1NQBH1Mr0GM0Xqwt5wsOwZgT6a0avHX1ZhIGzwGZyIIyk7m06zppn297prmpJNExSC994fdJF89YXwfL8wp2UldwGlFeWC%2BIls1I4zRw0yJch%2FdbLFo%2B5SEayQ5Yc3ArEJ03iaLukVHDT6ACYXyro1X8dylz%2FAcyzEOcf6mDaP7cvOvVjeEsMDyNr5xTUE1L1KKw7yeBf8Q9A20cKEscwg8DFxgY6pgGIVL%2F8LM1VBX3ccgonCIfLhRUzPO3WjEoelNDoNm6%2B3Faeov3TN04EaxfMQXG2jTZ03UR1vRYQj6zHhsJSCTWssoz4FdeOjMQNUYQWBAEgeB5zIAcK%2BX20kuJIBCRpkG6313V9RU6QTfiYe5hFKbb%2FaSWig5uejGOnCjVGbZxhgG4%2FQilp%2FFd8hPsXgVM20ewMS%2FG1yk65TRZF8wOC4JA3Pro%2BVCd5&X-Amz-Signature=57e5c0e1e455032d901d6e51befe22455f6124dc455c019bf0003e4f86f7659b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

