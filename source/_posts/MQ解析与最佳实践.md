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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y3ZTOAG2%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T230043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH4aCXVzLXdlc3QtMiJHMEUCIHC2y1X11gp46P0%2BiS0mxgqHLthyi5mjMq%2FpyEqjxW6wAiEAolhALmpbJEoC8R8nXUTa7wD7HaW6MBwuwidHjEcItZ4q%2FwMIRxAAGgw2Mzc0MjMxODM4MDUiDFxPqo7zYUliNUcSgyrcA0ae0VCbNuSABK4J%2FbPXY0anGmWKS%2FogGoJeahoFKP6mjM%2Fr46mdsQJD7gd3kah4UwyhGasoY33dQOpkYqLzttnBv1dYTolHuvhk%2Bnj1IbZXkwOrgSwAtS1ixnHKlDv1FgJWu%2FV%2B%2FS95y%2FvWI5wz8EpAF6SkikDWZxV7GvX5beDP7Mt4fsbCspGK5dGBf%2FoPTip0UzGzkv0vmlFQtrD6%2BOZeCIKU3jUqnGswPNoP57Vxx1e00D%2BF9CVSEZwBM7CPCPMIW5uVq80i1KCrSBQTVZfmxdJcRjinM9AGOXIKJxtZWLRK7Diq71oXfx9aVdtPcazE0RrYSi6rBKIrk37wovw7%2FtQ%2FtDUKuvERHSnhAsXg2TGJKmBud37gU%2FMpx%2B2apVMHW4MvjNwhvQC%2Ftg95zXuxCVLqUIXrjNLcPf7W5fDsSW7ocjMAdBWpg%2BaA5uZyrXzOrhG9t0gz%2B%2F6DsTt16QS0oMwjlykXGEbaO1epW0WAbrZgC%2FKRPiqCqKTGpJa%2FBMjWg0YqZQ9jpuR83ewTVYNxI%2BBdIklQph9tCvHfhK%2Bp%2FCqAZ8wpDxwOm5TdaBtwb2GAc5EYdRscwvA%2FCPVplDkPuQ0rLvNvMA9KrZjKKLyJW7ODvvz2afhpdoMvMKCJjskGOqUBbd9pBzkYoaXWnk%2BRZyI3Di%2FctG9kT3T5%2BAB%2FgffSSx9QNlNum36CbKDW4GPwaARIwNFc9CdWb6XgASljo4bUoaKdYF2EuTl4UwfiZUL2BRRuh7DtZBaiO6xKT63wSWwjPa23DWIfmRIuxD6oK1WqhbZZiHCxgnYZaAhW8nGRSq9rWTDUQBYb2sxZ%2FPT%2FpUgbQRQN%2BABVgqIcED4Q8DaLSMaFgIU1&X-Amz-Signature=569d3154cb1b3d5071b542b06e30d01eea08c1d626cc5a28572f6bcdc17a6a55&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

