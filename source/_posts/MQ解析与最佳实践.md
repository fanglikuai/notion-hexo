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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662IZJHYXM%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T160044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQC3o4eBw0ADH5wOHuwwAtwR0VNl8XZ2aGXM1i7O9Bw51QIgVaswmA6PsDHISm%2BNJtu1Sg6JwxmTbkS0oR9vHhUgthsq%2FwMIMRAAGgw2Mzc0MjMxODM4MDUiDLnCfo6UfnVyszG4nSrcA2qx66I%2F1ILaF4xMm43FyFaAKdjMELSaa3bF6Qscj5qVlHJ1PEmdVLfZmQb8m%2B77FkKoZaV7fJ6yhyQSDsAOQUoJriTh%2Fqm6sgCBZ4UmD2Qnfqn0nqzjvnZdRqqM6z6EncWFehP7x0i%2B7c0XnQpjzPgI37ZevE4hYbDq4SiOoPg%2FwFTselfbA1Czz0Tmjxd00CgGoAEe94ialBQRa0oPrg5oww19GaI094qgcg%2Fy1E37uoN0m65JFAS9NJVgUcLJfyKMZz0psP0OTI3LHlffshfaRntQNXLnvMcgPNbDatHL8lHNNN8hthAoTJUbzKSnxIeKPU%2BskNeR%2FifKsoVlMnsDhHzQBE6T7J%2FtyJiaJoP6UyHo3A1CxM27fShkXKAeoHSuJl2P%2BW%2FAhP%2BBYVcrJ26G40vA%2Bjj%2B0KeylYYql%2FFKsJpQVQ342dEDDAzzR2CmIuGrzF1NPjZWr0rbQHpzKf0xJHZHBPKYYXVKosrxtmZLgjzbWTNRZd2C2OPXamucQtorQ%2F4Grqzjw5aCV2jqyN4zdhnbF7tIgboflnCl3uJKQ2SgadVeTym%2FlcdXJXazSN8cnyMx9yyFG2cqVbKmVRi5L%2F89xc1hZu4TQHMxc4by%2BGHoXga2mq5weHIBMIDQmMgGOqUBIyer2v3Dw576I3KSIWLZSAHUXOSTM5g11Rq7eJv9xd%2FIC5GsxlEqDOhF6ADgdbDG9pa7PmwtV2GeYCbfNxtK6%2Bs5HwITB854zz1Mng9mlTD9RQtH3VO%2FDNex6DMw17moJFla1atQ1UdCD0ZAgpW8t8oEaehSq5FENRCZ3WGlWzqc9CCo66IDo72u8IyuMVH6rslhYjHFv94CAtu3Sc%2BcUwCwfVkL&X-Amz-Signature=98194f9700a7d20db2e633a550194b86d231dd4d772130eca0999fc41c0d8dfa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

