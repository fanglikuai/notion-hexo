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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XKTR7F6W%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T160053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHAaCXVzLXdlc3QtMiJIMEYCIQCK1F8i7WPteYiDI6H%2FaIzYN58lf4DIQHFueLv62hTQRAIhAM7IyAFSsMxezgnsUQLEpltc%2Bwlhb49l4M%2BPJU%2Bnf2SYKv8DCDkQABoMNjM3NDIzMTgzODA1IgxG6ASkocpbXHCM8o4q3AMsVX8JuZxuh6C%2FxkL957ONwbfplOsC4mNfxNO3e0f9NyCGoFwp5%2B8J33Je%2BeliRELLw3hmH8XVq18YcMe3VowOMh9rYbGzK6CNjov5UK%2FcJxk62cj4%2F6HgJ1FAdaIwWeVB0qT5slphjKhy68ev1J2XGhxl6bTDbBovewdRP7yuPikGTEaZMbQ3IHM98K6%2FObOFwyyGef5jyet%2FBiDqddswd36W0k38UQ5wUKYU0lu1INAS61drSaKBmvPe2T7tTPamvZx07AvYrWHrnNhq%2FNftCtWMM3UzXCcS8UoTre9OWrftFgi%2BgPbXOSUd0dS4wR6VnVZomC%2FMC0pvWkZ7GOZTOg79YO6SsmHmqJhZLIBOApke32C9vvrun%2BYZ71gl2eCLCLGwvJBYYcdeO0ggaAystd62liTQoCrTPWrPiVsT2sfY2BL5jYQb2enqP4v6x5BIDq7RIQlf4BOTOL6h40ZDi0BDBMwgUla9Jbzm7JBNzabv4Ys6IGwtWdIuF13OGQud9lIKk8UnFaSkaKg3ekEMwrGt9e2ZRiTlfYz3FLf2OhgimoY9ygGb7Jr96emgBwd9sjCuS8TMJYhLsLXLxmvcJLDoUW3foVwH4ilUjFRLoMIJi3xVqkl6UOPKPDCc3NLIBjqkAZ2yGNc17XUo%2F%2FNLRNLQ503leI3S%2Bao1bWKd7a9FEWgyVWO4F6rNj6QxLK2vwdjZ2KbeL5RiVjDprRXV3AOxM61hKoV9xOY9hzKgxAhl3vAo2GyPWoF2EZXyuG9B3%2BFF4QP5s%2FFSW7iYLqc%2FTlr0ab06H7EF7GUZENf%2FN5t0wW%2BpOWWNVAqtgGQfCfFaeMQ8BHDqnY0iwE8CynySrtYxsZgegv4V&X-Amz-Signature=eb70a58900aaa1276e4f32ea21fc19a3952fd6837aae2d0ff68059f1d3458b8e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

