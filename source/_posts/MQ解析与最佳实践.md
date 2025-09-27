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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666TFG4LAJ%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T070042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJHMEUCIB0J1JyBk1yT1h8DUc921bjBxnMsLcdhLPR4QOnywYVAAiEA5WjW3S2OWsVwP6PAXpZB53LJHDNjkwmg7gNHRdK6qWIqiAQInv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBG7xPE573zwhIk3OircAxDrjqpos%2Fz9rlFM%2FDGv4R38W4VXdVuCfXhLIt%2FGg8GbTLEl%2B2FP8lsJ3cvL2NqNRU0z%2FJOdagD%2BZ8c%2Bgo07DXrsUXWvflBstVQ0dG6Ljb1jCSXVsPkffCrvkkmWmTgEqoypIK%2FixOgp27VrWVRr5oGDC9xUFNJ3EMndRVOLC25VYQyCEv4lptricSjfekrpp7100tRF1I8Gi0QpbnO1emSkDSeKqiBq3yj%2B38e6ilQlSRSmo6jtPTOhJNksSTvwxx2Y5YcWPInq2mZXx1DtMdgX68lxaPZetU1yEswYTf%2F5PfGKiNYUEQ6%2F8%2Br0JXezvLMAPINciikTADDvpNVnjA32vDs%2F8MAq01%2BLDt%2F9h%2F9vcU99Pw2mNNRP8%2B%2FBMaZRJJwBUfNIErxr4ifJhDhKarh1422eoQbJYBcq6gl694%2FLKwBsPCxV3i7qQcn7IZAMrpaymjWVArXLcMkT53PXvZTA8keaDp58HzvMdNbFQ4KaykM93AFBUcuxqrK7tO0l2lC2JSjfXO%2BMFEXrJATshoL1xV%2FNbAOsWIfavqMXekRwUbPtYOrV4jvkAqe9DU83RQ5gxPfbQ0y8Ubojqn7Jvr%2Fk9uqYADaUAWmFwnqC%2BDJm7yN2TCGlfY7me60WMLHc3cYGOqUBWjLVvShKcfGtDWBMIGG7s7LhNkYWybb0nojnnDd8ziWbYENtPou1nJnijOWf%2FnG3%2B%2FyPVGfwZ%2Fv6oAq92JyF2qAfRCqJQzpW9AWkK8UtolDnyIb6uUUIJXbqebaKRT5WAbhnZHcnuUX9G6tCg8yLphgEL4w1N6LA0GPDlnO%2BDy%2FowuZm3TGFLp40eCMg3usOq5Yco8LG5QuP0PZqYpcf4JbrZcpl&X-Amz-Signature=80de40a42ad76bd63ba3d94498736370dc5c405cb87dc86ff24f3733d7bfda51&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

