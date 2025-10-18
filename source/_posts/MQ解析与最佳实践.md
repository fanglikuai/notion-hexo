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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664RDPZLEC%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJHMEUCIE3G1OLwOOKV8TJlXiwGHovSXhl9ueKqGJLI73dlzkJ9AiEAlFBHR4goH8AVVZb%2Fx0au1RZTE7TqBvBU0EE7HSHx7bcqiAQIxP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAOHFmiWp%2B7PKsevSSrcA9mbMu8%2BIUveVXEAy7udUjBdEOnhqHY8OQH6qTvOX%2FoYEbDh8WF87L1P7hrCoMEcl0k88LCqx2Pi75DZy6RWUWBiWD00XGPCADbM3huPFUHDhr50b2IotrqnJqiiU4X0v9MdMTMWKmuWXjTpg%2FUvvP%2BTNJ9ftgVDFmc%2FG64i%2FQ%2B%2ByVhkhhk9qvl%2Fbc2hLIgpOtXU668RaMuEke6PffhvqcO5HCWAJbNAiMU%2BOdo8nwsyOglbpMxVcue2JL4gD5AHTMJ6sJhzKMef%2BI3j6hL13FQrlzL9LgTDTEnDDb%2F%2Fus%2F93hr9sjYjDo7%2FDty1aQkOCSLn9q94CS31Pd0St734sfYiWAuxEAhdUodxt%2BWyPiu%2FK7leuTkIRc1M20WyftI5TCuqgClg3xH6C%2BbreVb1Gg4SMca%2Bj6p6a5E9KGOB2vmNqoB6RupxALtj7PGfRL8QXDqq8Szw6X8NdtzqL2RlHPJBYM6Agx%2FNPKUI%2FXdhd8YChWGg%2BbJvmDgB9Dk4F%2FB8eVwHcbgT%2Fkxdx9STTPUIwU0mSaCF28PTLrR0JNiNa90ys61H5fEk59uyME0ooCBx1sL3Kzux4L22VK%2F2fw7Q0yMFgTUraVSSZUvaKpDEEXx9JRBtXXHt6gk%2FLncJMI%2FJz8cGOqUBm6DZkzciWF84LRpljRMXUR%2Fqa31cNMdibBy739PEOxI2wEvionnQcFoWJH8UnjihoQklkRrV9PJ3sZe6tdOQ5s2Ao3%2FYcoWnbMILhootlOfulE5xC%2FIw5%2FQDKGtDJ%2FfzFfa33C9fUaUl233TLhPruCOlwEUjYzTuUH3ilXbyHaubCbp2kTBk54m%2BM1wsrOjiD%2FyhjUe8HKM7ZgDbiMo14LlmzuU0&X-Amz-Signature=175671d0fac348f07075a2fb1a2b18f18dbc3a0f29f09b70ec42b2912b6c502c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

