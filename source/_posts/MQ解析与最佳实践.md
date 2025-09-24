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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RTPFGKMA%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T180048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIG4DPatU9%2F7xa6Bu893p%2B2nc0igsCJLSgj1LIVvTIrAmAiEA%2Brlymp8YihmKJ6SQ%2FzwhIujG%2Bj3SAOGXSdg5zi3MdgUq%2FwMIYxAAGgw2Mzc0MjMxODM4MDUiDBhpXFNab8JcfUGiFSrcA7wy1heiiR%2BzyGZDqsCBaxCtGtVS%2FZYw44PbBEidCPzBNtXcmuOHU3jz33VmdsoOmQcaUFUJ2VFAnsS2SuS5Hg8sSSGLiSTWXjRfycXGRuckde%2BN5S%2Fkqo%2Fr15in8nCVJEezzMKPxBZQuk9v%2B7qIIMpbgCPBws5dVOfwtYuHGfM5V5sttwiCGiuXd%2BSXmuDjGbJQCmNZE2%2BBPkIAq2QN0f3uDqwp8dxUAc3RQbPXX5cZSJLLpQmjfqxJDAhKOuXt1p1H2Jwqv0UdQbjVQ%2F75p8IBN2X%2BYpVSstCJkXJ2tpQ8xT%2FOIkVcgzDSxMp2j%2FlDDxtehCQJjCP2ayRReTl3XBev%2F4t3h2zJ%2FH1gu28SwS%2FNNY65%2FmwDEpEgyeMhL61sZ04JMoDG7kaX7bdqZ5r5LTFEMXhA%2FB0c65XZ09puvCNRO%2B5YRWoW3nJ6LwxBS3um6W3dCpb%2BMwjzkBGcRI9jX6TMrukfoMkQF%2BAEQBzN2Omis6Ica4kjU3FqpI5OkdRIZ2E57aAqmQ%2F5a%2B1MNK%2B4io8vRxCitZU%2BPdzTDK7E1MBhiec2rGljzJJAOt4Q7eCJFMn%2B1BYscsRlscPn%2BN1a0VQXwBx6hCaLVzVA2GKKhR607qlq8wyCqG75El7cMI3d0MYGOqUB5kQfaaG7Nk4ULFhhip%2FhnbTsDu1D%2FoTFFFrSAJ%2FU%2F7CEcRT%2FBuYIvfVxyt0GTwvUhe1zQrwCemY%2F2ewG1lr6tOmJGHJ%2FcWy3%2FuiItwjIeSoZdAF9SY5yVlp37AuejVQ33XCmbNseahfdG6Yq7uz9lQ%2FOkzYQjaAXLou1Va4j27uMUL2OjSbUNeMpHVxMMpZP%2FrvNTSa0eHwcB9pAlRfDXVhlTY7a&X-Amz-Signature=8d4e2d45b6d60c7e0a600fcdb001cfdb6085ba19383e93b157be6d2463389510&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

