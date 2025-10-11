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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667MCMKTKO%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T140103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIGsBUnJjE6kMYAgltCfDkf6vf9GRemXVM8ElCDi1m8nAAiEA2JT99P4GN6vElkz1oGslhf%2F3ve3vekI8w7Hbs0Fs%2Fhsq%2FwMIFhAAGgw2Mzc0MjMxODM4MDUiDGkxngAW0zQUbrmdsyrcA6XD1RVPey%2FR8DXKtZIm705%2FBuat3TGo%2FcG2punQbQGRmy28nfSNLOfCCVU2BAP%2F5cGNEj5MAEr1VAfvmV86AvsA0kXw1ggp2VHH3I0A%2Bv3Fu3biqEk44gg4aGcz7d3UXXeUda2t29qrUADLagTqIn6FkX8BiB0NYJlsJzXPIbnr%2FILCy387FwGk4gNu%2BzNLO2i%2Bzvb0xY5WYWHsgXI6fooHjtIMic97D7TyVnrsn0Avk5A8uG%2BFsvN0HV0MMUqMUmGsaV9yrbm1BPAKBuBmfFzz2pxFlmrbRPs1Q70e5LIcAp1iizqiCcVp%2BPh3TG9apphoZQS45VJOAKBXng6HaiXMDb7UiHtsRCB6yRhFZQ5r4zKNa0F6%2FChcZErX%2FpjsNDDSxHE0xFf43z%2FDrr8K%2FB%2FRtVDmUym2gVgyomAQv0acvL3mfQu5KOYgdgUXs2G2QHZ1snDUM9DvYrZkWiJ%2BU8bjX5Kdn5qb%2BLvQ41WZdnHT2MElxALXpPTkkrMtp6dWq3E2aMsJ2HEzKoMT5Bz9XC7VvXKGbGibF8%2FpKuYXPOzP5Pemp1x4ypnZaCa4nJbY0JY1nbfVzJ17%2FZP2w6Jpu0fd3hKYQ8EdXC9cISpWBikt%2BUPycZBbozbaHdXmMJ%2BlqccGOqUBiNW%2FtuqVdzISyjgXzG%2BYMWsvi%2Bn8t9EYn28Gl29vM41y9w9gOMUpBZO4c0HUhhP4ZHrMLGF0uO%2FqVVU8oq8Q654KzLDbA%2BTm59ng7d%2FWZS4wdjEVbQ3gHnJwZCTIvUIvchV0yvQ4x21DW74tsSBryO6i3fcFgXewZX5eGDInFMCtjTQdBaxkiKTndRaUfUslSZLVs30hU%2Fo1Fqeh7shEJYpaxmrH&X-Amz-Signature=cfba10cdb97cf2bc8e6583400df12dc198a1b338ace0208f643467aa97f90833&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

