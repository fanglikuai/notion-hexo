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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XM56SZN6%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T020049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJHMEUCICCRQywGn33J3ZCF5SyvCgQPhD4BJoH3Nvm1tZ%2BVMe7tAiEA7TTyGUhWU%2FyZQzHNtJkqxoZaWKD%2Bm5O9Y8dmQw499wMqiAQImP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCTHg8yRexoPLWHUxSrcAxrXKOpIMa5OMhgd2LcLoyNw%2FUJhUW4GAieI%2BBeg6L8FHf61T85OGJnMAlnivzXbdWVTTPZ9iTaLI9ZjDGgg9SJxfPDBlpKD29P%2BcLvgcicJfEwJbopAgEFFSH4bsn635qGWlPcWJXNXQQ8I7EiatEAZxEUWB4%2FHFoCNbAYrWZJpVm3LLFjPxHWw2RvcUwmktIe3CBZlFTbrMnEXGRgAuDDIVZWgstoq%2BmNKPsHDtyTtaoMVTt0PS0x8cutTWn%2Bnld2kEaiS0fUesIaKI6Sb%2Fn1rfntbA0wXevHQODGJF56Ho8ugtR30OgSSPQ7fcnGHiULw%2FYaRz6qg7ae7cSXf4zwM58B%2FKwXh%2FWt5SDow0LpZ4zBTi9KcvrruNyU%2BaRev%2Fzc%2FTNq20fOqLN7xxOosN4sknDFYqxAbbNPUY%2FHqLCLdiCA2i1a50Kit52kiIxWMthwlAf4ezLNLpwqqy3q0i9zTYF%2FujnwaxpWVfworg3oxwJaJSSxZQd7AnwI%2FW6A4vWQhv2WJhl6g8n0QyDucNSzcJ5gQuomM37n%2FiCga%2Fw1qWdqiBZVsYtu9J8ezygklIujUI1PBuD0chsvdFOgac8gNvs4pcLidbS4EMBE0iMPAqDJ13oL%2FUeqA2Y0fMP%2Fbp8YGOqUBZnwxdBNmckSE3dazoH2r6Kkx%2BGfpWX83PnFTxLuwXdCWkwjXbwblXaH%2B4ALtZCSJ6i0Nlpm%2BpM5BATsAfeJfJICVmlNmQeZJw5XWOBCBwvqQeAjYf2n1RXzEE9YimSXmLaMuzbut5CT2bvpBzOcmG6GGL3F5TmaljtfqkPhkKeifulycE2os7DcL8WBOymIUFppCHz9uhxtBEDn52kSJl1oY2WYP&X-Amz-Signature=888abe9fb520c886ee44223eb665209ee90e10d9b388ce7ce50e5eda2804e243&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

