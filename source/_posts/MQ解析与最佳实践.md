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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SKG5TXZA%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T160044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDr2a624LuCI6mpxruWwqi3Z%2F04VVIAwzbabwg%2Frjx4BQIgDunuG8zESg2gT9x4nW3jU6WITO4LLDXF4CmSmt4q3Zsq%2FwMIYRAAGgw2Mzc0MjMxODM4MDUiDL9N16zzKkk186ayFSrcAxF5iSSoALQhija48FImru%2FlD7AH89i6B%2FZfL5hS20RzT5BPOxUs9NcHzniFh5PBmbYZNuZ0bUvqELid80DSknBcFtJ%2BgSjXf%2Frf%2F%2FngyTNe0T%2Fcw7u%2BLhHilW%2FUHcdiSOtlsXXqZdj5ZuRBlKticCRZ6WjxQEMrWxPWMWT5614MIF44ngDOexPFXpgm8lPFp1tPB2jjKJkXZ5IAytupyR0hPc%2BTsZgWGLTh6TnhuA2haLgHctkuMjx8PocTbe6lQs%2BunR0MNMXk7koMAkpx8WAzQwUIlk%2FPC9iFmBRm3kdWY9Bvb3wT85oGt%2BP8xbFA4lkFGfQk56QZlzqP3RDQ4O3C9BnF8NW0OyKb%2FOdfPHqTnnx9TI%2BNzEO2QpWEFgjyAlxllUCm01288JMkG2oNWCjKgrEsUDlmdrqfPCxVZZkiKeqCq8frHUgVPRMO6KAAbXna6Bt8FOd2W2GmlnwlnP6lbDuKHtK0ZeQ45zS7l7Tz%2F3SoPDc2PjYDglzp5G9E20RAHrrh8geKSeWmv9BfXIdi6smHVEBQTjWsjqRudey1KoNs53in7Sfjziu38%2BDQ8lhQ6TCvGPaoxLbhec4mCkmTyPtDIt0NdGcjjZycGrdLLWpuSFdn%2FSYbAINPMIeEhccGOqUBlZ%2Btiiwq%2BggRhlCxz%2BOgbvPZOf3tdgheDZswsJNON0HhEzkTpblxC9RNsSxLKn4PGKm5iGTvBapXio%2Fs1ooCHdo1BfVKNxFq4rv8tAZqepR699PAZXFp13lC8nt%2B58HLUGqu3GQQgzRq1QXoIX795DTUWGhcQLY%2FISxFYKAFAVWByLGGCdK4UAvcZf01u%2BvTkwXttGSqHWtb0EOxcD8Q42Zoy6Kx&X-Amz-Signature=18d8735d2824ccb706f1586c833159ce35508eb004c1390511a45d8fd702632c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

