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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UGQUK3M4%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T190040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEsaCXVzLXdlc3QtMiJIMEYCIQCqkQfxjMBaB%2B%2F4CZvp%2FvzxNHuGXajc%2BMg9oJE7Xub60gIhAKPG2H0s7bG%2FgxTUHogeT%2FzJl0RNf0aBOljaem7dtqBkKv8DCBQQABoMNjM3NDIzMTgzODA1IgxhO9UhfDvEpG1VoNsq3AP%2BLaVvfKhQtgbeNxVsvyw4CmXDriDtEygYAKAydG7yhXKO8AbEuE0xnFRUSzhgQQH2lvb6fwkTEFdvSgMiEv84YUSkqs0DYceybYtz0fAuDWN%2F9msOPA585bTbDmHcbAKN69DPdG81baMpEDQRHyBN4%2BnBaxDZ3XEr9xlwBo4wp8XURUS0Vo%2F%2BLa2HfloKaC6PioxnxMVjf79otLrpulPVun1xZj6xPb8yDX60SoMGRyEBs9Py7yy4P9ZzCxDrxIWdTJWj9WbrTviamOnR52sfpjEVCl916ZwoVxjJIG23bju%2BNacXbj5SwbVlKkNaiK2MiwN9lzTC20UL0HZMib3rwiV2wK2CzXZvJuUGbtBboGeNpIItat2jn9aO2o1cjmSwGRdlZG9DuYABGcdKbkQStrr3axxHUVrrbiXlbo6itgolSFNYR58MNO4GTUKBz6F1HmHOF1oLf7HvKOpIiIHxDllsmg7pwo7AIOmVOkaqwMAnBt6LWg3QRv2RDU2TWW6785Y8EwdRO1UwvLCB69zkzQp3JfcQNKYcBAwFvmf5RFzh1oS02b0N67WuUzQzC67mW9oLkot1ahMpjt1g3TpEGTa%2BFm9rCp2WWJpWhh%2B7%2FhQ8tnxs93EewzQbOjCh6YLJBjqkAS9FeRiQbKBS9MDpmgQk96LZx9aH44NCaJN3AxfQwJhpyGLi6Z9fr0XagVD34HA71FvltZJysrqik0I1FkCM2okvPxdG3e2EIo6%2BFsNmBTdtEEDX%2FCx5H%2BQCFl7FiFccUeAQSAvkby%2FyE8etuxjJmluTZ7TPmC4ZoW6Bd%2BMZNs110KY3Y5hZ5WmtbrqEkWb1DUHlwcZQL1QHlkY%2FGEiGW%2BR1M3Bm&X-Amz-Signature=aac3637ec676733ccd74af9b7533966a8ffd58124503277013fcaa13c3404394&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

