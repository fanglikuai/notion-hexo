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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ZGZL7UW%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T050042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJGMEQCIE802NWvexTIBp84KGx6sNhpsHu%2BVT2FwGMOdIy6wAuFAiB0xn1OAmmJCIg6kb7SGtvva9wCtFkFPr8Qdl2FjYR93ir%2FAwg1EAAaDDYzNzQyMzE4MzgwNSIMRZyVUNmpcZz4baWLKtwDVSqh%2BiyPgp4BcDUjZVu%2Ft5oO5WEeeZm8PZyeGiEmsoyHxe8uTNZz6ga%2B4gPE%2Fjp51k4Rfby7HonTTll5Oq%2BXg4qLdX5Gse6dq3fMWqSCrarknTVHYaNOOukYIDog1paPaEQXMMUnmP9nsoVCwldCupDJfhAu1q%2B326Dy01y%2F2HehK0OcSiV4%2FoCr88w0zmS0gHbyxlMoZZxcXPRHEAOnZ7ALQoxW%2B1zo4hqpBxozIpHs3SzjJKuqdC8u5TFMcOMa7eFBpkruo8piFZ3RzuzZyuxNOLhMgx%2F0mf%2BMnq8YGdOaLDa4g7mP5aST6aIqyZr4pRrSzjTBhykuclc%2BVooBQguprCxZ%2BzlCTxG13E852SkecvYPPsYqPCTOxDBVPMUSopccCwt4ekGteuHLFnPJJBqB1523Z1gDLiz0Piz0Va2jvlTClwcIfZpM4TkXsEGbv2d47h6Bl2uxzvOcpkyiEuBtHVpc0N2c%2FoFoJXVA8cyTqJ8FphhCzvbzghmj2T%2Br5P61cxOV5xU1%2BNjVzon9Zad%2FdJRtU8%2Fkxs3uqLtVnAs9cEfslSq4law1EiCNHsRaFkXMV7qr7PRkbuKIYGBOfQJVDhv6SPmmAIyMddTp44XqWLo2gyTHJLy%2BEcwwm4%2BKyQY6pgGCoEbE5A0xozLDJklfQ9KI8sxjQOqm1YUCAwZaAeV3QhEqarrF1mmshADv5FffMc%2Fzq9kDNRbFe78RLUl%2F3lhg%2BoC4ZqaJU%2FOLiSncMSD08b5bquKPB6ZwUp9I3VP3QvYh1SoeMP0TkXeDKgO0Qx9DIRH8TKu5VBfM9Enw%2FY9ezjRgbXb3%2BaPvcc5TtWIjNTyalr0RaicYVjrsQ84hUMVQlP%2F9CfHa&X-Amz-Signature=7dba7685ba37b605fcb7cbc95a886613120ff54cfcf5ddc1831c2b906d0acb89&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

