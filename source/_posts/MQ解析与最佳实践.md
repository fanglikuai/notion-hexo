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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VFIOGEYO%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJIMEYCIQCPDhD5PraYDJj8hZh4QQpnsyzxW6vOjSX%2FjTTYAkpiIgIhAK%2FK0iQfPu42s6%2FcnHrraSk%2F44J6z6bJXJfmCvtejZA4KogECLL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyCH%2F%2B8vWQCWMaTeKsq3AMQR3gldFWdK9WmdVN7VD6x939wdFiWtkWgSCmVcSkweBtVQnycAG09LclvV3A2XerKCxgxRVSaOuExOgh2Dx569k6L10XyYEgMhnfkwMq5V73PV4kR09sdU5LnUjn%2BYe%2BGTZw615fxrZrT%2FxwOxrgXzbCnmAK%2BI2MDE%2FNlah0nQ4GQ0qNNoWqvf2qisDDUPMZA5pg61VhQOC18eOxJDyoIXE6nSruHIHYrBWzX1w9P9sYBFqMkD9fWc%2BgdZb6HZ7bt9O6v0KvOqrtyQ5pfHUVqg1ixsPrNKgtVE%2FwAodzZdaEXTr%2BkJayywF9zQipy15s%2FwdCv6Xz1sZr1TZh0IZmIK5GxRDch2dEuqJiQN2CpRRdnEckx6V0SSNGSOWYiAJMq%2FqeRcq7BEGk9Yr%2FSSZsJgaYonGGSozBicJ1Roi6Dq4aT%2FHqLta9Y1o9VlOALhXDhNQLUYPy0OvJETdajNKQULygFZSAgHMlFA4RmBiaW58n4CPq2GUvM%2BzMObfg9LA6Zmi3%2FnjQhz6Z2wsCBecz7MSX%2FbRSxuOUzas5bDdrCSh740FX%2BQA%2B9iPl14b6KR7%2B5xCtQWD8%2F2DojmbqWkGbDzZdS6fdwi9Mi8jNsoeP%2BQxaa%2FSvNAPHBUfSrDjDRmuLGBjqkAQ0Lid2xyU9IP%2FxOzRTS3qsBx43mJo7PNk2ocuj7gBCyP%2BWIigvOTfQPDm7aKgDpIE%2FJ2XEUXLIWtRMKl61OtJKJXHz3MrrlBQKqP8WlzL0yibDe143gtbshrF9EOthyE11LVAAM178QbPztiHupOL8isKbrMOXnuteflPpgdUtmtKxCbvf3rVxk%2FTUk6sgelYMRLuzGBNYybUuEpnACjtAPi1P%2F&X-Amz-Signature=669eb9f39e8099f0de89914193bd2554cfd38c98bfa571afbe85e3ea724100c6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

