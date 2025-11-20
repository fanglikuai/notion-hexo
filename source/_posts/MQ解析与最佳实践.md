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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XJBM75ZF%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T200047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDQaCXVzLXdlc3QtMiJHMEUCIQCoE1fNhWBtnWC0JHmQzLoM8S92itYuQzuN6Ofp5ilC4AIgC3s%2FuzvZwZGaVeJa%2FDKAs%2FQsLTuNOPsriL7ARvQGGgIqiAQI%2Ff%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBKCwINty%2Fsgw2xmGCrcAznTi2STrikFVGxLReelVpLUyJLH6NxhBjTJ%2BHEnmJwJnWBSorpkc4m5EBS%2FxL0OEg7T%2B9xC%2FLpLOgGs1Sfxf%2BOdC7c%2Fx5VbgtEqt9w%2FTjvKY6SYIIm5mpoARKeRn%2BATB2lIZSrFQ6uZUHPggMu665zcDQ%2FCrENkNPhPIfGGcDn39NO1prTfhXjWN1np11r%2FwiPrIqQrllBjLC%2F3%2FWMspZ3H%2Fr%2BLndrs5jWaJ2Fjy4ljC4fi%2FsR4z12wC1jvt7DouTOl25jymuhDnAV3sFNdicX6UGwOvKeY9TwgC5n45I8oh7WayLjHI%2FYwSUYOq7rXGS%2BpZGhtVC4aRjl3h%2FuF0eh1UoHV4sK7kxuvdvZ29C7XMYosCi4G3RGZaSEIIWLLBLijS%2FNB9QoxieuQffdM2hU6tCuEj2okaxOUQahcyTpqilp%2BwuwwdGGfEINRVfqgFJ0TYJGAa8iwuFfwQw8zEUM65z38pp0yh0%2FEa1SYh%2B2uZhCKRPsv%2Bz3alFACCn2%2FiDre%2Fp8E1deYFeZ72WAWxwN0FZvGKZMYmTcdIXiqC3kYrdoifmIEcc%2F3VPR9mbV71Ks7mS8qePCdaTHYrkj06jNH2pLrdjRWi0B5xcH7778yom5FfYi3PZc1g1tGMLbh%2FcgGOqUB5lPvMAO9mOE1VtWDG%2BGXTN0zFfnu1S3%2F91liaO9CZWZbPpHqYOKXEVXX1wNOrYQs7rz39OKg8b8RQfHSfrZwzbp5YN9uo4ec81aPhClMIHTRsxXXeK63V%2BxXEla7M6A8QqQUzFNlLFT3NiPy9pTENmNeY%2Bw27%2FTAivmjRCDQN78iZOw0NBUdhiJpHnxtKFlbLynwELYsTgwvmiq121NVAolexkg5&X-Amz-Signature=d1924f2f658979e2c5a3db07dfde86f0a45fea90dbddb29c1c694915fe8dc658&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

