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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46633NQSE4D%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T110044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAc1SXEt52kyj0%2BZS%2BX0xXh%2Fp6EZzQQur4Zdnfqmjfg3AiBPOIFTzbU7sxWMhOoHaIm3jS62LyVXEo67ODGroLYLBir%2FAwhEEAAaDDYzNzQyMzE4MzgwNSIMeIRzVUrjAp3lyNERKtwDfMrOx9cZVgoBjg7h7dCmia9GIhvHzF5xv%2FSZouI1uml8OrnMfGmY7PnBAUZDry%2B9ioo4n7oX%2F5L%2Fe0PaZaFW0IGx4sJwa%2FZwrvvyK%2F9pJDGMEMcqV24nmAg9WtEqkc6vb%2FLoleAQRS8fTlxsdsdfV%2Bkz1PicrdsIP4JXEW%2BBcl9w6oj6ysFKIdstCWvWDtK07SX%2BrshFuh9Hl70Ev%2FYt8lIl%2FpLFlKlH6rHzpv%2FrD4QxgGar5L8XCWdZvwsuJl1Iu%2BAk1EdWQdFjimXZ3ZPvhMDgcAdvqdXklnVHEDz%2BVOAweFqylqS0950Y68W3hib0Xtg90r3NlUjR4kxGsXUJSyTZBhTofRpemo43sgZou9pRZTPYfGnlGeDlwZiK9qA3gLOnkMVzmpjFj2GzuMWth0iMVwJSH%2BlmsP%2BcnvplKd8fIP2K1pO%2BFcOF0cyF8vDm6WQNc7jM1prhLzROEzKFQ0PDgFiMZOkwH5AF3rPwEYg6An%2BUUHp55NsmR8yP87jf6%2F34reeVSfQXIpAKNa56tDUWByj5g%2FBrbEmwoZpiLO%2FMXJ1ZFrfsvfc2ou5Mhvm7tCvps%2FWQuvRxfmX0JX0I0%2FGrgJ0bCrR%2BTdkNl5EkSTAnEo1BdhL698ks4TQwhc%2F%2BxgY6pgHTtWBW2eGbi2%2Fbsvm6MCMu28x5h81ur79XeIxO1m8uhxvpsun5nLk4%2F1EDgMvzBnF8OZMdpBbVlmGqhtquxFovIkuhlHATLpA0DRA0su%2FOMIqthFQq72ImVA9DTD%2B9xPTFhxnkTEKmUXE92zRcXsbJ1lJ%2FEMCTQ3rJB6yCAr%2FFr2wwttnmQZl6cjan%2Bk09q%2BmNQx7aeNjhBeS%2Fg%2B4J2dPS3R4vde5K&X-Amz-Signature=aed9197932e3bc2b86da1fa5c76bd708649d69f791de00b80c036715727fc1c2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

