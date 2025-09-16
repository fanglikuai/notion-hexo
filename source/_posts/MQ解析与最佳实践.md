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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667NS4BNMB%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T180044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIQDizhGb5SnEUhJBSW%2BHrIVpYDNueJ7nER%2FHsTmkT%2F07hAIgdjBpU0vo61KmlyHvuJLxjXLjjtJSuXAA0ShmZCTfam4qiAQIk%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDrdOI6oLQ%2Fr2w%2FKVSrcA7kRm3akmp5NwsF67MjNL%2BhU6eNUEp139Tq2nBAe378YXcbhzKTUjExGdDqy9Ir0Z%2FkCCB2jIBklSiQf2Ple9HNtP6EMLwU3ztdR8IEujD31A2Un1IxgDhpkoiIpf9K9C3lAqBiNqoIW7YTBy1D3pDdpZ%2FkqT6yAocU7b6TJDuWYzNCYPDHmcBoJZ7NUyNfQGTv4EMPHawerasnuD4DankhWurKl68TfFSd7UjnexV45LL1hhBE3C0Sr2hHx9Slfu3JobkIep8dinTV27ttnH5E%2B1BQFMcl8y1Srf1iyLxD%2BXJ5goeP1eCuX1LgMKl%2Boa5e4sn4NAGpWAnfxZUHh4OmZXp9wyM3gMccK%2FyHCZq%2F8JzeBYOqisEH2KTbmIUqSeo4HTVuHCLj0IT57uygD47%2FdEN4UBrx64sx%2BC%2FaSlhcKm%2BBR%2F1gSSSlDNPZDAfg2EkFiq12iXCiUhIeJD2Q41t1ePAJ4hNsGHCRL9JhScFS1V%2FiSko3Uaj4TZBrsyxLArHC8vtBTue4y4fG0vQfZQ5UDi16M%2FazFFDOHnc469p9cMW6c%2FyUzs%2Bbm9aDuZaK6pEkBP1mISXLbzaDtC8rWSpU5kecJcmpBmarKLLHP7Vtx2kGBxfC7m0w4fU%2BmMIzDpsYGOqUBsSonoYimvFQCUXS8ArkwvZGPpADwki%2B60Snv%2Ftujud5r%2BCRyJTskCk%2BZfGSsRq0lzhBs89QLzTrrjY%2B2c49C8Ix94b90KkSBSSh97RfaVmOEweZhFg9AojJqXZdTPEvayH%2By03ZALw7JAbu1RtTrCHm1D5hG7eypqRUo2luKw7Bbb%2F%2FmmSoISbHxgjSgTHJ1%2BXPJFH3T64k7i6m5p0Q38ztASD8B&X-Amz-Signature=d89dec865dd5e441274c5b2826db819389df7045531489a7103f4f42373ded55&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

