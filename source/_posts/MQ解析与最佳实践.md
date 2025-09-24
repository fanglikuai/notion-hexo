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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TVZS26XH%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T080105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBNmdvE8%2BUY%2FU4Aminu%2BWpdgb%2FOw%2BZ5bVKcwwLZFXSyqAiEAkp59CJGq9qpvaPnqKp4D6Goh0ltMipImsV5UmAw%2BaEAq%2FwMIWBAAGgw2Mzc0MjMxODM4MDUiDCCQDJWo8jmtsmNrYircA3YEqbUBNQ9VlBM0IFEEO%2BbxK6AdZG%2BOszzNAJPnbQX12bULP2spBp291PFwdu4UsRIopkeTfAnLzQyT4KWTDNeVtx3MhuIpGL72kY2BAVtsfXSdrNjpZnVD0DitrDOCy1MfSlTbi8Nd%2FT5i%2Ba7JMVzKuJtOihrzW1xmjRjb1ANBheCMGQL1HyALqEG0ezuE8GJEHMSo6AxpiCqndq%2Ff3%2FTzrvgQ%2FnH6j3D0zLDC4TBvje6z6XC%2F5BToskA%2BhaCPj6eY8LvQMZyXONibSqXBK7z3AF80tvfmTzyMxjJngYJwyz4MUhzpCx0xYT2kn%2BWZjWBTAHoyn7YLD7Jz7V2xTREyYHntuUcb5KqocJ8RKGeIvSQc2IWPKSosKx0A18KxI11GClPOzE%2BCXAL5j3Mgyo3YB%2F7gPgYBa1%2BqcmexXJeqZGdJrvUZrycdwMh47aE6zpN4%2BX5aPqIyco2c8rHGbnyqpKcoVOZ5bhN4MPPeukou0DGhdduA5jQRak%2F29DgX4JOOcA5I3d2NeQMI03PNWSEEOJUPDGSkG6x5%2BrapldcoFg%2F%2B23orTg7SUA0h4sNVtyRxXmSBi1quCZl7lfkle%2BLgUUJW2TH3opWUAaEW9NqGId2caDuxGv%2F89%2BocMMq1zsYGOqUBfH8NGXmS1TPQ4h%2BfQZesEhOssf17WC4WD8NbxWijhZFVCwwzlpa3l853xCqC12kdjqgqhYHxQ7azhr2Ou8apbMsIYh3XxT%2FZpXQ1t8y36DsMQg4cU9AmQq0SJeUlspEV%2FyPzx92HJem%2B5azQcAP%2BklsrdxRQT7RCGIOjZWzZlIBkzyZeIPhxuj6XwDWMsfX85yp%2FKAY6JZNqot%2BMlxV1gBjQ%2Bupw&X-Amz-Signature=88af784641475b64a23f84befd58acf092893d155f35636d56065a831a1bd703&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

