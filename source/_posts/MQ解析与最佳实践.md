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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XT5WQ7PC%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T110044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGcwtBm4m2rzNxRTXmsa5a73bf5yICm8%2FeXJlCohyAe4AiEA8ewWGgSoUQ0Zcz21J6vurjg%2B63vc8%2FMG0cHOGqc0byEqiAQIvP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHxH3oBCpRDaDgCypCrcA6gHEESsMZhwh4b880uCF%2BIVAP2rAD1u%2F3kcUb%2FmRE3NP6LuY2RQaH8nExhAN%2Bo1iRUZcUFRF02SWpXDTZ%2FaC0gd5F16ruwQ5toNoCRwMWBARo9rYDood7l8LSUsRCwOFnO6Y4bDQ%2BdrvefWvT3YwdLZVP9M3cB27UBOy%2B6gzsfDNf57RUJ2ODya9p20jw4JRPfOPc2pn%2Fer%2BfVz6C5gcy%2FA976f9CyZIKUuvhhUtj3HhLe4a8ztt%2BsOciHGFcE8QPIgLIgj1xwUm%2FaiqlPZzovIAO7%2FOkLP0LXO9BPmNySS8EB4SlbS0JpcXzW3uTmlIUdgPexgPavFX4voXwQaAhLU62haiXbrgB5xiPVu3ReElN6OnD61A%2FQEaIPDMWfafhkEKLHq%2BWVWzGetbFBwwH8WbNZl73%2BsGsBtJY5h7bNZYJcO1Q6%2FIdJMuOtM7RccrKMFEuEOjRghvS30bIwvcBpDlKjAbSWXAUj7orja1fl7RM%2Bvaa7plmzcRZGakXi32fADMZ5WOHe837WHpIlOD78siPZNGZ%2Fc7hch3hXA1OfGSVUXWRtjqIDFBo0GBKeRet6Rwq3XBnE%2FygXKqPEtOkovxdm2H5cgiccNxFbzSeIa5yljRJ9%2BevLbZSbGMPmQt8gGOqUBuqJ%2BdNhvy0QDrkS2l0jucC626XA0Y6qEZSIrSUWdTM677HCL6nGTT4pLLNC6pqvMTyxuai3CPebioW0jMGzFsZ8RrMNKrJwYJ16GgbOViECbIa%2BTGxOXtoU8375tHlF1iZMIhW%2FGzm2epz7494I9m%2F%2FEU9LjF6YLkAOI3UbT8lnqwKRtnx0iFzax%2FYmsjW6y40tvTbX2TAKo%2F8A39Xs7nKFaN4pO&X-Amz-Signature=8af3d3124529c68387498d57bd8bc1a7eafd1e74654bc483a66ecf9bf8193214&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

