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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZJY7ADQG%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T160045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAExFR96dtswfURqJJEjGmWs69GK5JZzeRAZPI94z%2FryAiEAgsrklYaEfqTUnc92HdRxfbuap07Uv8wzUgkbP3KmblgqiAQIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOvVXUR%2FuIrNg7CAwSrcA1wleMv6YcK8hGHZUqy6a26NsiZGI7Phs3aGFDtPmdeJMN8bFp1i4LQymUF1eK7NzD8jiRffWbR99vwStsQktCPd%2F9fZmQSHMkRRXGEdLKHKsoqGIH4tJe240TT%2FwxlgnP%2BM8Jzb5x8rhTXENZLOnCnsv15a2do73wzMqLls9TnhQ8rMCpmG%2BQAA%2FV1J4e3jun0mEF9ro7LIZelDhkvHDGAeJvx8%2Bmrhf2dD8XkEtR2ciUiRr6n%2BgQpofrG%2FAQNupAzK8EwECdWOfu0%2BZWskSONcmi1%2Ft9eAKquO4rgrLIgjeGzyZEoL4gsJzhw6SDJMi0Lty2v5%2BifyaeaQqbsUw293ezTx%2BHlgV1xSrH5Sqpyq2VbA65DnThw%2FnPpimtC5MIkFUc76NOTg352gcb2NQttHpz5nmSWkex%2B%2BICHOOq0GkgYJGKYvxLDpUowUgKOFta%2FJxqL28nyJZluzPDRaWe3RWc2t%2FvIs79PQbbQmiEEBGImGn5kwV8%2Bc6TyP7TVEufEVImHE1tOMnpZg7nsTKQYXgcoVOsWh%2FULMoS233674YZTA0iBzHM31w83woXwCs1qej5RAp8Bb9xH1310dvbuUrGC0yZotGCRxMWiTT8EPCnDaY3scN2EmE69UMNDX%2BMcGOqUBOdVjnGL%2Frm0JA5o%2BbRKQ6wyy9vC8Usc4Vh%2FZXiQG4C9xPq7nbjbCTBBpmJZsLxmLenH8d6w2sUFLle%2BI%2FuJdmrhTknDYa7exwSfE8rBvaDY6cbggepZe1K0PqpdB3nrJnyt8R%2BfD0cCqLtLA4eveHHn8007p3YW1pSQS3NxoBaQYrd%2B1nHrTDKkACpKF3P7nMO%2FbOLj6eAw3Y5vZztqYEO3Bmwt0&X-Amz-Signature=95af68c46c1c7730886735d36c9600a97ee6951b88f71482c8c353f295d24026&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

