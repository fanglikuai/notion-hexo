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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TD5WR6KY%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T180039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHHxy1PzUqS%2F4hBNvSpSdpUMR7N3USlT73ieAM%2FAD4uvAiEA465k3sNB6wxOaCSbn3l0TGpaPbei688FlDAZrJPyoTIq%2FwMIYxAAGgw2Mzc0MjMxODM4MDUiDFBByUxTnZHdsHC5OCrcA0ltRfosXiNirPzSWDu%2B%2Ba8vCxQVe7LPfVko7OWw9uQSyXCfZdXvv%2BxeRuByStPlhfiuOAFaTvcDQqxDqc%2FRk1O%2FLs7HsZmpXV2%2Br1glopqpgfyTK6Sru%2B85Y%2FvGoIcXNc6g6fAO6%2B6S5EsElDEH7n7dn7KupcDmgo%2BAJqwiFtqnRllGwUK6EmHth70118yeODcVc0dYmlarw7qE7UVNhXiMkZnOhlFvQPgzfma5yhliv6hVg2L3iZC8F%2FxAg7s10oqWUdf6rWMtlxoV0rxozjZ4jaYr3pYT2OvUtr18Rh8K5iM9xsyVwBz8uAvCSgWK3BBRu0eo12kGikdt9aIKveMk7CAeXuPn9q6F2MiQ6rZTOrWfzwSQJtalh9ZpezneA3MbTfijs0OmZ%2BXmoEEFnkTUmYWKhZEV6VaJhmxyXcD2AYW%2BSq1ioeeyjPmvd%2BDcMBdJrmkfDMPc4BwRC02CmSaXVVPVL7HlGxE5TxVpByWEEkbIKLZmV13DA6tupjhq0QbaV7df1xqc4NDaFjQSWMdU5EU7eZ1x6oi0QRl7Y%2FRiTQcDJDY729AIRgPO%2Bt3Iwzq8tsymkztOWcMywzEFHtSpu6JcQ8swrq60eK0klC3oE39yQyfjZmidtz3fMMnQo8gGOqUBiUpSomUNnK1Z%2Fp7HzATSEUl9mK4JisKmgK%2FJSTRqIQm1GricSKOmgoqmloecpT0fQIfACZF%2BZ1tKJUee5qlWAcrUH0Sx6TpK9fy%2BWsCtjhlEwVA3qcBFH2jieM2TNld147ulOkiqau4g%2BmuICnEJVYoIhSXEqh6Oa%2BVxWNGRptLvIP7%2BYR7bFIcAYOlJ4qWvArbnsjV%2FLPW7jmB2HFbyihnmGm45&X-Amz-Signature=8d918059c0bed729f790710964c2b10de091b1a1b2ebe0a2762659b823c5b8c8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

