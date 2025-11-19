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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666OPBI6DU%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T060037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA4aCXVzLXdlc3QtMiJHMEUCIQDdkqyVfBgMo8pVaPErocfCUCnppKOPrOcdjQr6p0sHMgIgCciwBCvSAqgoSJZJsp44lRVbQrt2%2FrmXNcuZadOGNSgqiAQI1%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGAR89Cmi5zO9QunmyrcA2n9G8uyVj6h8HYCHL74Mnny7zH8eNZs5B6l60WvW1aoQFpa4VuhiYu5mfAOZwH%2B%2FrPQM9oV7EwgZQcLQGqpRdRE1NEofPE14iJbWyuBLkCC%2BUO2tEZz83Yd%2FYjjprib%2BhxeY%2BRyZjZyv230bnPBq8C8H%2FuWXqwU5k8KPfs1S5MuZ4X0CR3ppXOMwuMxEO7GaoMrYJlaFX7BPf%2BNvW4j%2BsrY5tlKSryMqfXkxZzn4shtpAR4XavMhpgNzIzA9HSASZTzN%2BXP0CUPfclv1upFB2HaBb33OjwWXuOEn7kqWms9kD6HNtR%2FtFuyMi0FhsJuWco0KyQL%2BBk5uJQSV7BVzR92jmtQfpfscoD1nmxZ3DrEqEjb4PaxXB0KdkLQac3WMnkUa4DJ8R%2BEYQF0OM9WSnBUsfA7NX2ewSewSmognrFuIbJefFwVEd2Ccrcj832nrngqwuF2rhk9PBWmyl2P0fdFaFPavSAum9J32B3aPwXJmP48oSSwWI4TcMmfNwqJxCVgUo1EX8GfocTEtOqUSCb4W9ylHS2oLInrxEjI1LMOI3YAxMLCQ0vimQVcFX6C0%2BQRTqPECGe9Euf1PkvUX%2BcyBXtj3WnQHQUjILLO3hsmzaMFmPLhSnFNarxcMJGz9cgGOqUBRsBNwp8IqHz0hipGqavWcWKleqol6rFXzUQf95%2B%2FjOgOFvGgXTE7oyozPrQK3jkB%2FQ6KSLkAYYigFwwW2Y%2B%2BvE22EvuwbEo6dXwSXHgcQI7AUawIvjTcq2fYHPNsSdY9jSJ6sFmeyZo4yj8clw4zz7X1TPTKDO203KAUKnWR5ZeQjTuf3HmYZJBAQ3r0SAEvibVZ8U8x8ZQyl60Sh05NGYcUUhqV&X-Amz-Signature=2c951bc804b601a938ec2f0f246ec63bc53dea57013ced379eb931dab565aa81&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

