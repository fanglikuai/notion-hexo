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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46644AKDUEG%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T090059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAShc7hiAnnw2F%2BN6%2FKn5R10zWXiStN18HVO2wkrsKIrAiBTqX21Zrg7sWK3a2vJTlgiRT89fAF40lPghJd1O9AENir%2FAwhBEAAaDDYzNzQyMzE4MzgwNSIMwzkKEwRJpg3LcbVsKtwDTyyEBLMgWR70bNMt%2BfSuziWdPcVoDDn0aBJx%2Fxb0aq8b2%2FZy%2FJ48ibTzAnmbKf%2FVVxPL%2BELLbYXr%2F1yjUn0Y0L8QXFHkP%2F7TdwSZL686xvDaprJebpi7M0nNip%2FFuIBZCDv%2FSfohkohkLtLfIBR6111QUB0EaBkbJCsOEfgZv84X6AV9vIXszVLP0yr1SguSZMylxiPZT5zThP%2FB6gkoDewOymqSEax3AE%2FCBLhq7zGdmyfEgVVtfzcXEOCW5hr9%2F0lJPux2anJPETA8s8HKrxjCfTyXFWL3gWxkl2TeQopTdv9OBngIdp1DIAL%2FCt46n5nLtgy5DgpQ9BEqLNOVg9BtwH5ddFaaZuwq4GGaBIiPJX4Iay%2FmvY6p37j1MSDVIekkjb1rWs1W4SVyoRUzvwB4BULd2mvUouPWBfg9YB41ejrkWoy3DQLq17strpyy%2BdnFCsVP%2BaAQdpXD56AW9Ve%2B4PJXYbzTURMvCJbE04LuIzUEolm2fJI22iHeq3pmZiiOxNDiZmqKqkI9kbwshkrRY0zMgSOTtD0P4%2FOeRh8fWLNB6Wl%2Bc2IaqtCGB4PGs08uJHF40o4kPeemRIwR5pxAhr8YaF9aQKhiIQY66JVSPOX2VvwQYdgf%2B3gwk%2BGyxwY6pgH57W8jDkz1uBj%2FZ1zQEz3YLsw%2FEhghiya9YDmF2TRToO206li5rWJPiUq18jtU5e32kGz1B4%2BkjFepkV3bzsyCgFFKaIe6tB%2FTxfMlp%2BAuM%2BERC74bb06jpd5WCOzMDSp9kmEIx7VAvh8XIg4wK25pMoiotGkr5OTSeRElTADDVJOGPtSkSqx9kL8wWeaO941bTAURhosgJPEwumNCBsuHBn%2FfAidv&X-Amz-Signature=988556f7cfced48f4b2593c604034cca4ff517b62a357b41c38f12a4745e7c7f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

