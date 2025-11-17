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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667X36XURC%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T050048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDDNQNRG5YKOzcc0fR%2BviyjgNeXi4btqudwQWb8nqV6iQIhANXi0X3Dyg%2Bquq2lAomC9LqRvU%2Fl2I0wFqajs9nnkvFWKogECKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyNB2zodrtkxzMGaNQq3AP3p%2FOKxSPb6ah%2BMbqlSbJKPntvTDjXa8vIRBSEWgtAHMmAYPau4pf3GIYzXWXwaKGpI2FRo8WTjjdzwLmK%2Fw66tMQa4G2VAjBC%2BdLQPTafDB3EBw30N6unS98MTpkhdtGPUcJBh3Np7v0hoecKHagJu%2BB10wVHkrmKovn2riE2SuN%2BbysJ67y4ksTdxmJ5p7Vtq7ZTY9NCg4h02SR6R9XD1fuGfBAm1jNAduzmgMuANR33Y%2BsW61Uds9NhBOr5egZml%2BJrArJTly7H%2F6EuIFqCWvNUa48ah%2BMBPQAPH9Mn0UgRIxLCE4Ry0Y%2B6NaBqe7h5ppLJsD5JxgbTqaqPPXsXrEAc6XeXT986u4v93zN%2Bg%2BaE%2FEnMN5GKujRS9S%2BcZkstujTb2GdQGcCY8aJw%2FzMs055rAy%2FbeifPfe%2B1EmEM3GaJ7FVLxmRvUzqkXvXquYxkyxVbaMqCs8cJPl%2FtiHvoVIZW7c2muySUpXFCD23vxTEhT7F%2B%2FRqN9jYt6enqccRy2arHBKmdAT2nTXqxvnXcGQgwL2viTqOGQxwwm2yJcQFzpIQVc8t%2FP3LO56wdh%2FrtjpQsqurs972cNbr6IMYEl%2B0CRgZ8iGu%2F5hw8%2FhVr3keNh29dkEblf6EXijDJwOrIBjqkAYxCrzwNDYklXGVejsF6b1SpRiWn30R4f7uOLushPZGcK2aFa%2BCHKVoo%2BRZJVqQWpLflHvKM7qu6KSx6zUFvo6HAssokpvd16jpqmGKVhLLvybSgD31BlsD1fls7%2FjVqYWmoXoBP2tTP3F6ba0xJu7SM6vzL%2Fy2uPQxXbk%2FGDDSbea%2Fi7Pk9oflXIlQs4E4CQ7vLko1OTOMLIfQ9V4w0TxR5yyEh&X-Amz-Signature=4e2054d50e5577a0524cbe90459b6c757bc752961489ab9df28dcd07dddd8e84&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

