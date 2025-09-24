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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664WK67PVG%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T200054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHNuvQgpEnQEhO9oUj8fAbffVofVJElcCBjt4YWNfugrAiAX2zobOJj5BBNq8IYByWBVyY1hFJLTPvNospcQScYc7yr%2FAwhkEAAaDDYzNzQyMzE4MzgwNSIMuybhBTeRftw1V3pdKtwDprN%2FxWErAzvQKkOu0LUadl0xmDuryS5N%2F8ESc2EHGhH45ZqW96Y%2FrYWF3ToxISmH0O5QyO7el1U9t%2BTO0mDTVfbUp1ISZZXq9iSeSh4tEDVkNmSqP2MlixpCRwvpWMDuezDszeJnk0lFwh1tu%2BHUzWAIdlHWSImH3nDxcpWdqdT%2Ful%2FpPc8DgS%2BgHtzvGSXZDcMTAdjiY%2BMcRBH6CKYDkN5BNZR58fRx0gue90F1zZDun3fjysS8wqQ9hSpcJwwnYDrIX9tEmcdnDBZOnVFD%2BKdA3MVtN2IsIH4%2F2g0L9Y%2BIhxuCiELZ6IRWRy1INQmSvWYcKW8AFytVDrN4XlR7TS9Da5vVXVcJrWeUGIGlpT%2FJoSCi%2FrvchZxVzrBMKFGx18m%2FaWwww%2BIpTi3Php6qTV%2BWMZXSgh559cAnZnN%2FN7y%2F%2BxLkZUe31gZuwKivNXOB09gNT10k0kF2aA6EeF2vJqTSrGkvVV0NsFIEAIDTX9FL1rpqFYPBCcrVzyrjngthPqD8n%2F3zn3Dc3ot786AX3ZMbkCpbFW%2BJphmG5bP6SSLm4x16Z82t%2FZGnK8wnORHzBRCVa%2FUr76KKL4SRo6dlcptBSyN20aJRV5vh8G%2FMdI4R1K6owWJhIOlhiAsw7P3QxgY6pgFm7LtjPeByGMm9S2rBg4vok6IoA5uiuDL8Iw%2F%2FEsQDH4qzGSfxO7pLP9tG5GRDP07ATxjGbF%2BkOVeNgfyvLB8IS%2Fb9yjB%2FNDDcPWz7CLrqJ%2BDIwmT9NZozrAhYe6KstoF%2FTsQ89IhDjXXqbu9l%2FS8gfMn0L%2FqmpZ7Oro4s%2FjyBi01qgZN6AosBFe%2B8fjnO8iaTLTHkpebhNPQ2zCGKwt24uCnr%2BeCi&X-Amz-Signature=e4518a99cbe85b9bc70c0fdd203cf87ddc693c1283793f48adf04f91e8f23716&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

