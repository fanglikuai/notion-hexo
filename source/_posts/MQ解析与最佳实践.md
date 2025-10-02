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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ROFCBIJI%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T160051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDSTQ9aM3f4M0JXEO4Y%2B95uTlxCATrdrgPZlQgM1xtcMAIgSQI5kZJvn8SFeHr1S4WM3nb5Yi71AtOI4MrxQkQ0Yuwq%2FwMIMBAAGgw2Mzc0MjMxODM4MDUiDHMulOPTQhLi7x%2BegSrcAyQspAeD601F8yEhnC4mmFcVC4kT4sIV9DtW4nCchbCGRqlRtxpvvqMrdKNqHQ9AuoOfUDMos4o5BZvlW9BfNU1V%2BXrDaQesFPVE9NubKlKo8sfW43iRfoqsqm%2FrUNdmci1PCmnOjXlKy8xeCYforagt5XTFottJSMCiAer8k%2BxK40Y3JEN5CiEeklEbEm%2BZ1NH3oyJ5ZQRpEtweiNj65juIgu1TISSXPgMkDT93ynJSkMrnoCNvkYWvCB8q4yxnYwsyXWnDisQX98HCenxI27EbMNSh6z%2FoOPDfSu2ddtMNMGsIQqGFyeDo%2FaCxcmXH1ZIvzJk5RYSCFK7ZSTaSvjqxj6H0h0Xbf4M%2F0GTf0kouD7rchgDN81hh7foNbmieHP%2BoPCuUlCUVnpNgKpygTdY7Ngiww0QarGGDxBw1ujOzsTpvB9pWceFxtJRw%2BRhZ67u8Hf%2Bdmw3QsRca%2B5g%2B39ZpYTKWdtEKyGUff7vdD2wVqU8wE8FkmWVBHz3xutbNpAKifpHeb4%2FQP8it1S6VhciZZGXecIbsUxNwoYx3yfEJqOoFgzBElKSxPS8zvQciikZNuw4tt4xyptkxtXFMqX4QqXBld%2FV5X8F3VPKOL6hnvORlx1dwUfG72qnFMPms%2BsYGOqUBryHE2wPsAeZ07ghnv4JEVApN2Cbvqcm%2FsZ1Bba7Bb1pm8N3E6lElXkIhay%2FmqPEZwUTGtF7huXhPAm%2FEuZxYoR2WsbSs9bym%2FF587exEX6gE006F9lbiiUhTFHhQIq8rvAPfD0j1e3mO5zujCJ5sS%2FQ%2BQeIA7YsC%2F9WQDDqILXqA73hpQ5o1uJDNaJrFhgJvQr%2FpL4umpd05d5xS7A1MbnodNQ0l&X-Amz-Signature=3e751d20fae252953119aeacb5311e00a805dfa0dcd9eafe73212fe7e908223b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

