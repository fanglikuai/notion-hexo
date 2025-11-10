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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZM5XL6SG%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T080055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJHMEUCIQCOujSzxtEUyDhsQ6vs5%2BxikXEia3wsejTkOdiNCu9bGAIgauj4dAe31bn2p2xWtohsecbuMwnfwRqTmpyEw0ItHG8q%2FwMIABAAGgw2Mzc0MjMxODM4MDUiDIG4DQ68s9LLFkVUWircA2j2fOP0aQaxGZPU1jUBnTAGH%2F%2FZWoE%2Bt7w2c6vz%2BdG%2BXpt5zS7elTbQ9WZY8jPkmLkDcCh7njoqASNqRN70eV%2B7xj0cRZbimT0BYimUAYdE6JP5fC6v9WZgJWrW25Gsn5Tu241YO3qzH8WR%2FGQVNuD78je4FhW5xPKqXXN9anv21ithkedst48kVUxg7dfXKI%2Buw4eH%2F58%2FkvCMoWd6IL2Afb0s2Bevg5sBmGIIwXfgD15hjv%2FdUL6L%2BVhqaOAFDfrLF1dH%2FlH8OdzzC8Dvivkwizc%2Bv%2Fu%2FksrtErslAnuaOCzFaz3cnFE0grYD06wYLBvt4%2B9CEGagJrWFuIGyOUfFigQqBH12ZiX2659mkWqyMcC3J2M%2F9kiGpJaes0aCri1eodgvunx7NIcXVRbcMPo6nAJ4ow%2B7JY05MaiRphYbCI2SluBisf7U4z0bDq9PxM6cxYN%2FBUtSMpnw6FNhnRtNFAxdFDwlMLhI9UaZVgms2OLpYLcy8ojJ2wylX%2BbCuRvy4Zx5PlXWlV%2Bvy3%2FjI4CbN4G4r0TY7ZBNXB%2BgE3SSbDaGHyO5xqv8ScvwsrFvsFXq0uiL3PqXE8WmtF1ZR8BnXb4Id0wig17Tgy8ha0xzB4U2sCglyP8sUR4pMLGYxsgGOqUBc3lwpWvrnKaBB2a3NAfkZyqoOW4QYbLSfClkB9HaRA0psO3IjOFepxJLlJO9kUey2mEu4ceysmP0t7E6rwDLs4ZaNZjbH%2F%2BCz2mZXCXIE%2FOVDrWWMChgzzISFXvi3m1%2F8Dyp1jdvTlzha9wUSRt4NIuTzRjDt4xnAAPxBWo6SbmxV%2BaVkcpUvdp5%2BFdqbyeFJ%2FbBMqeKswjgRIVDPvmDxbjx%2Fhsk&X-Amz-Signature=7dd76d669e287cefb73f77990f2b26260183620ec6f813e04fd9315767b863b3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

