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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQ42PURU%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T060038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCyff120Hs7fsVJVX8gsUf9ohDCtIpYvolY7LkSNHayMgIhAMBHpMSn2zxp5MsCeXPq7fV4Wqxainp%2Bga0ckfb17v6ZKogECIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwlco0gkYHDR%2BSBcRwq3ANfhg%2B9tF0iUBmKD7B5Ml94lX9vxwjBFWB%2BRDHxKH%2FNU9pWUsJdSUrJyKOnLGEoRUHmeGEEfk%2BttmVG0A7f6QZgB2gwhw4NrHuR0aQEBV%2B4oLPMUT3qU%2BDGlx4X6HRMsvu2RWISzRNjf3Bo08e8HZuKMUgdabM0IaYfG9ObwMX30LgceoMzi8VulTCToBkMwZlPQN807Bzl5AMgiTMbmMg4wbGlq%2Be1PXecrqharQuGn8T0DDQkEcemqUX022qLtilXWyHVnkIPuCJ4aDUUk9Tq%2FVmyUZKb%2FUKojomLjiJI6G8GNpFR5EO%2FtCSRGQCenJfg4TpCCgtKTp%2BnZPH0sqa1zS2HTR%2FYmPbVEqvBR5QqG8Gzue%2FoIFLCWx4Atukm%2FapU4dMhzDmva37jLcYIoYlrqLbAyy6JcnN24LFqZomGoP1IdNSb%2Fg1ELWVq3NFjmf0sGLcpeNcB50L7wiUCNwdRqAK%2F%2BLxQERPvNftF1yrQRjiZYjQv0V6ckNVs1oZMbtIdgO3HDO%2FTysuTsr%2Bu%2FZ2U3ou83bdYag8FbU3FNuqVbe4eqPGsobT3CidoodvRKp204JRk9hR3D4O68kmoD9AWblWfr7TJCS1Y1Mp7bRmcgb9XnYhnl1T45hOeLDDvzuTIBjqkAX32iVyxmhH5nL89p5Z6lUqI%2FbdkJ5o69Ya5VC2kNwlQbb11U8aX7WOWA5U9s5LhSq56wAzClUjRl2r9QlFEv9XG8N03qxwUXPm%2F4%2FEehiA6mdhLhDL2EKsMthfdESnNEGb8S%2BAK%2FTJcuTgA7%2FzmFML04R7SVCHo9qPaAwaWo4WI0BFQUXdUqeZNbvBw2W%2BsGexsjE4INzjcz7De6HmOMUtvOTRd&X-Amz-Signature=3e86c105bc3550a0872245741acf8ebd4415ab4487cd43a7161781c9925073fa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

