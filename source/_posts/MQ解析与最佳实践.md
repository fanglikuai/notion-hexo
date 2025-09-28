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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ZW74EFQ%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T140057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJIMEYCIQCNRnl100aDxHM1t9cnWMdFXaHqjYVlRQzvoOEhzdJRUQIhALcn%2F1zbz7PDXLEfrAdLXnu9X5%2F0l2Cory9QLzvh69%2F2KogECL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxqekgZj4EWuA0B2mgq3AMf0nMdD0qXm6fCDdMzb4ResG9xt7tS4G%2B9Rj7UHV4ykobRdQdFwrdFskk8JFZBTPrdxxQn5OCf29pV44mN%2FvWXMj0ST9EUwqqAtcxM%2Bj4bJ4%2FEAKgihpnP%2Fa4QU4x%2FROLnBDpW5z9TueAElwfirdAuZtxbJY%2F1IOpOzX18NVSbNLfyaqdCI7eMbVzNuLlqQ%2B4A7%2BFiwU25h80yf%2FA%2F%2BmRm93JRpO191Br31gtj376uFiAUdzzj5qwZQbL%2FMPVOC8CnAw1kkgwrkd7aqnfGimzYsnGdXnAYbQHGSOFhsDExLCFCNmtyE97QX6QIEdNR8kspwJ5%2By%2FNEuDqoLJekBh9c%2BNHpF1htHyqburbObxlakOztRn4v%2FNY2cJAchXObC0emUqa9ABmi5k4%2FVYarXA7en2cJucLu2I8YQqQlYkEmvtVX37zfDeZpfQ6dUt5g6pQCOq6H9k%2BTbv5k9We9wgPqb1ohnHISIpfysyleUnolK3CozU0Q%2FL2QIOB4%2BMyYb50aFpnj1lpi4dUfXn28Olbi%2FY5Tfc7%2FN%2Fb5SAzMac4kY2XpYNAB%2BAFShicQK9CD5wXelbm6MWcR3a%2BkiPHC6vHxDAjTxYo1a2hexI7A4wVKa0f1aiqyN%2FVOcFdCmjCZ7%2BTGBjqkAY9DqvluwQ2brKh47%2FwWqTJf6F9FDMw9Z58hzwHp%2Bs6VWjuXqS1ZBZmDtoYhQ7GBMU4%2BDC95SK7%2FvnW0Bi8a%2BpJRi61AfqqUIjHlXrrxIfPk2htLRyo2us8xlbHo7EE4fy6C39q87FXMfEUtMtm2%2FpaOLdtEI22O%2F9Kbfph%2FfhFbNa47eH0VmNav61TpjpAplsGLpIzhAzeecBdFWuSLsIJYlkD%2F&X-Amz-Signature=0df7b205bb7021cb5ac7735dd3ac488a283906722b1d2d78a1d09063bc92180f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

