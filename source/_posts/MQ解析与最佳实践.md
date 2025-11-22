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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XU7R5ZAU%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T030049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJHMEUCIHJqH1Up1qYDgFjdxJ%2BU%2BLQNvbMTz3yRyTuSaoQHPYy7AiEAvlslvcuydaRhipj%2BPuKHSmpse4v0w3KC1%2F7QX2QLsMIq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDPH%2BaC1xdGFJvAnk7SrcA3uYcftsIW%2B%2BintdC9KvSQ1E%2Bj3hNBlOP%2FUHeIgQvTYmeKTMkjFJ5RwfAXGi79Ve87w4KVCzZBxyEjqETLlTQAur%2FJ%2BAid3NR%2Bp9V%2BAnHQnnUnN36Kt2mbPJzxpN1rpGTjibq%2B7JsZiPJyt4ErCm%2BbeXVl7Gvwqa7f1O%2BpLs6AkfVrNKeA9uuqTmRALCuycpB08uevQGUDE4y3hhNwCfj6uWxHckZvVzvS%2BDPBtVbG5gGl89%2FISdpBGuldOHnDfSG9EXg7FAPepaPb830I9f70ZIjif7G9kzsRSOACSVOsgPolGFNprLI4rcFWbtvz94QTBxyJpNHYoHHwn78PMrJj4tcJlCbhhXuhtwHR5d6WlYVaTcwPIAxKqiV1jsQilJhkVXwjf1kW05DzDAyDQ85t9D2ZBzGTBtyEVulz3lp%2FUx5sYIbV2jyN5LiIZv1Ke06aY2Wo%2F1YrcZBecIX9r4TcBXxDKVEKM8lvCVN7MILw53ND8oa1kRpufz81qeap2lNVeStvYEETJOpHDpp4JLC7brJflpfV%2BSzl5mxiJvdUImroxWo%2BKZNBNyt4iV7V7uYHf1MLA8gH22xTmNDUlsjTvtVGFuZczU8GzqwIfszQGL1h4uRKdgo2%2B9GzumMNPKhMkGOqUBsmw%2FtsXFhE198NZVbwvN7rrEMsDfwDBVvyfARUMKeVkJcpw9TKGJzCFxLFunkaaf7M9TqxLpzE9QiA%2BEBADY4SSn5lXx%2FMajmgmlP61cKHwGKYJYdfsk3T3YmA0XBmsgrCzVNwuI0vJHN8PC2bOp1JV3Nj%2BNoxhZxXcjFiNeiwNPYSFjgJUgsF47l%2BU5jhVH5zv8cd%2FvZESEu8EuD9vZGn%2BuI8xU&X-Amz-Signature=722f0998d2efb0c0ea589736695d8148280e6073173c99c3c5150150fcf18d48&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

