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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667BLTKDTU%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T120040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICkJFv5IdhHQBSLCNWCrnhaXHwv6K6PCWhen0ziDgyX1AiAPKS7ZpgiUfXKsE75JGfpq9VYSdVpS0nWJ7bS%2FwzBKCSqIBAiL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMSON%2F%2F90JzEls5dxlKtwDfWIcVSjwC5y9LJHWSJAkUblWoMc3eHHXhaBiV58iyU%2BZjRQDI66GEVO2IdyNaCO84U2jxk%2F9hxMapNS56BjrGgcGuSJfjL3%2FapaGgX4qRlWz%2BJlJKwGZNcrtFsH4PQxlMFf2PtMt1yfZVQWhEgebZUtGTdx6X99BekCOjZt%2Fc3R%2B2vcBrqcM4VSPONVOyeJGvwAwLq1PjVJ35Ia31PlM5EiuxYHpCsmTmw56Y%2FN6nrv1reIa96MBxm6R2Dno2omGbrkhITqLxwjUK9YZede52Gzz3S758%2ByXBEZ9wLcCuTSlYkk0R99TPMDELfvYcOUqeWReYfHHcxzTlkXlvgrbgOp%2BalZQx3t%2FcLu%2F9Vuo%2B7nf8wF8ei0lwmTihYZoL7HOaPf4T6CGgKj%2Bfpc2RdPmtuMYoCexxUe1bZ6nBHBdZuHTtAiK3vJ8QjNRHUQEYuVqNhu95NawKZgrVymnoNL9ofXXlJbjOLqK2YjUYzQ8Df80nKstF41zyiQ1sNRSx11yhsjd5qP9nfvSbuDxZ5AJyF93GdfEUQmtPKGbBq%2BVmWIEcajGNQ%2BEkKrr1sOfZDS4%2F8wkKMizN683iR%2FrKAVAbpYq%2B0ZsjAoo30NNmDXO5v72DzocNn5JeUhsX20wmq%2BOxwY6pgHfofBHlZdVjuIwPuajKZL8rDIZ6y4l14J9cqdcwZT6s65sHywj7ZFFA1Wf1HZnaQCAD6PqbED60loz5V3xjVPDICufVtwOHJVzwDhYDBZRBv9YMUL56qSLjc%2FIQ1SOBKwSceaHKM6xTyG%2F06wCtxvUEDq3m6%2BUlKF3kXZi1t4RdQyPPLF6hsJ2EuHJbvCxsUS1YQOuDGk6tFKyX%2BK3aDrURApMXn8j&X-Amz-Signature=175b87145328be6ace7373b544cb217ef62cd7cb3d1e2805efc27a38234c827f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

