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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U2W3XRD4%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJHMEUCIQCrY%2Faf6nKePgTMMWBEoYICWYAPKKuLEVEunVucmvOK%2FQIgaLzFqSRzo0IJg3nLWlJkZET4G4AM35MaapjEZbakoxoqiAQI6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFl9GPUQam7rIlAEHSrcAxSZv1Veaj%2FQd4ZGwj1VvZn%2FWT92FG8KvLVP2fTN27BTlcBulMC8X7a1XMlGKBkcZx4bhU7qkgfSy37VC4THiatQaz5a9HL5N%2FwuOay0qHjvfDABBCMoKNbDPtWeatCSB1JrLl4BJJxvaK9Mx1i9Ig6nF1oFy1H%2B%2BvgG5UEm%2BlFCKVpgPNYwOF4EpSh4ibxM05OQQ2n6P62rh6fCrwvD7sTxq%2FeHLweyFZznbm1An9B38p6ZQFZssLqQeCQzuNXZwCLV%2B5ClFZ%2FX49SvroXE1VZ0y3SGnohWtHdWV4jea7aly7MUu%2BY%2BZB85A7wcuiMqkpNAUNsgiHUzvuxhNinRZix7rP0sGHy82UCmXtvNkbSc6HlOKf8zI0KXqwhQAkbXuHG34vPcKK92QkFNC4YxeILh3hwrUByAJQ3fo8DIUGMJ8Ky3PteLK7XYklcGkB4gGW74Np%2FIJZJSGWpZY45lXsuKhch0qSCgBM7JJemVfvo88Ljnq3ngRbdCvGki4a1tCHvoZnE67J3UAkB35bQeEg%2BvfZt9Ke0hQTj5q%2FRwUxRqKc9Z3Lo8X9NkynjNwWmUBYCPWR1cgpZ%2Ba4VKibl9ewAiBe9LXRsZtcNWdw5%2Bj6ce%2BGkKMcSgY42Igj99MLHqucYGOqUBillWEu6%2F1Eyn4hCd84yL0yz%2Fg3CnebXFAWjDrhCRrMwXcfm%2BmNw1FQYK2%2B0W7GeoedhXf8eWrVHm9frB2s8N8uNKw2KkMh3bTN13vfu7ecmhLdedOFFi2NLnvW4I8Lf8yx5QMxmwpET79u%2FjvmAD6GjW9DkpBtrrHE9Y%2BJNI6%2FjkGlnZ1rMY4PDY%2B2ypSK9NhA7DRR%2FSfVuC1jt6C3Cl4I1U7RR9&X-Amz-Signature=be0ef2077d14a5a6833abf10ad24733444abc97e9f380728b0ae0b552f34d38b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

