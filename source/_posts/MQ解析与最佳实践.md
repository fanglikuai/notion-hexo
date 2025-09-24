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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QER3S5TG%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T110042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD5Vlf3Nv7pyOcev%2BvCgURIxy%2B%2B%2B0%2FfJfC3JZDr0t2alwIhALg9MRld48rDQJgbpWx7Udni68tFIAQi8Qmbc%2Fa8HpYjKv8DCFwQABoMNjM3NDIzMTgzODA1IgwC5K5TATTo5tZhXFMq3APmZN5iEl5zmJnOTqG377mM3Efd7KfgyrdQbLzXA3EemwnML6WBwhpoeXHRs%2FyBZQ0ATzkiaj5A682%2BcGDutsxswcsTYxN4EOf4t%2BzqGVtZeKTu2dgUqR8gVoa7OTvvRexgXdy6VkJIV1NAERXGWU9mP1dv8herVCf15VN5B6Aouzks%2Fbrbh8Z2GKzBaUbHgci%2BkTHAxkOHgFDNxkHQ7EWqnxuKCppmLF1YVCMD3miIdvzJtBIO3VBIlUSrRQb%2BNCfqqk9KVaXhFKuI27vHdOKBkLu5AgEU1LUSfp2eH6658AnLOKW%2BEmozx5jweU6PiFxdQPTEIsH%2B91ttF9CmEIrPtoEWSnTp1luX7xKBNDjWcBoxCy64Tur3oN9mWN0YAKSX97gVWmIAeKRya%2FovFTGV831SlRgr3xVMrXya9YOPs8S1zMxpP6G2ZaiU32gU9ekECuoLNJL43wmvzP6fev8BUfZQWGUVYnKT%2FTMoBM%2FLhrHnemBWevi%2BsVEEqvDTG0TMNax9fO3hmJcGKfcwzt1Y71hpCuEP19s%2FpfTHAvBXdtWa%2BP51j%2Bd%2FPcaL0WsVBB6%2FBNhjZ22VkZvrozOc6unM77HM3byugmEcASTkflewAl836Sn36XHxHO1VDjDflc%2FGBjqkAUegd7k2cuEtxyiWppBaK0agrZikyryC2LUhzS%2BVpJf5X%2FyEEDQIqpz8k9T5zKL%2FxwZ9B17sxGtEjFjeno1Qw7Ni9Kchd5AhATdFf1Z0fbrkGT7iFe8ca73t%2FR32map9b%2B%2BfeNbm%2FPXD4qpswyPl1GcOMfOo5dBHkU%2FNOKmLQmduC%2FquxybnSWiSApG54ASfqQrkJMDQpjMut724pU547lolxkfN&X-Amz-Signature=e0b9beefbb27d3053b54eec75fad8541e6ea9406369bab7d7dd986f220239cff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

