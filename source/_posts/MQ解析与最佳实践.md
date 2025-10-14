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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662AMP72RT%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T020052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCShs5XHSPE6PP8XqbF4HAX5C7HpmcsPN1axFxIDFFYzQIgDPSnLoIe7pkpokzU5EkZsuAOJUnc%2BrnIElqUu1wYNFgq%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDCWhRRb8T%2BSCjSUosCrcA0KXOvSGUMga1G3lbArB1YeSW9luJ4PX1uzS8HUfVcX3fLEkqk%2BkYIeQ2CyT8AxetIYVF6ovPBOQ6aH1NIjgKrsGOeY4TAdLL5gPjD%2FAm4gG9Zx%2Bw2PJDiVHcCXbQ1xwo5zRjm4wTkTOc2ATo0O%2FwECJ4HK4p%2FLAyGLUE52bXBuoiLeT4eIxV8Ab8F6dUCkHjPo%2BdepsfGtePKIXeU9A%2BEoID%2BfRXUujh%2Fujh1E%2BYJVccwdEG%2BClcLZEFDkpqm6wuxoz0NASVB3yg3wZFbfuTUuQfFE%2B71SKSYB%2B4Ng2E7bobNkV4HRXeXZiDyF5jr%2FAtEeTjb%2BOMyO4%2FYWsTvfGnJ5xYs4WIkWRpYs%2F99a%2FdVdimwBROwdvkm7JawbCdaNdKm2nVBUzDP7Fdbe4QosMoVW4IJbFH9EpjeWYq6THH6EsbarkCFhmOVQvuzkQUqXPXLhkO66nPYBsHbg2xqtxVcAZM98rwQrrt%2FE%2FQuKFWnXB%2BVMjee3s41FbUf%2BwQqHsxNThwrsP39Pfj7jyCOwpGHgFrdMRLFdUDm2eEeUHAKpkOTeQI92zPz727JpsRisay71Ja9%2FNBKgJH2jWyqUcowv7h9I%2FFqg1HzW%2FLEHa%2BTasrWSARBSRF0EhTs94MKm2tscGOqUBlD4QF1wlwRrDgnfKKT1wEC17%2B1v%2BtXJ5omo28nUAnbyMpEMFH3WiyNAjxlDytA34WoIKsqynXV6Koj3IRbvzY6HJwtRs2VCf9NDEs8onE5A%2B%2FoyM4qEVQZeTG5yqJef2SZ8%2FQoNPv0jTTt0YbZY1g37NqG5kk7ko1O7%2FadKLkp3%2B5OsDvsFvkjKZ%2BDZrczl3GfegAhnOJ6qb0kumDvH09X0fK9XR&X-Amz-Signature=a4762c4f4e0d4bcf19c5348aaf21f9ab0752bf561c72ddaf185a886f833ecd27&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

