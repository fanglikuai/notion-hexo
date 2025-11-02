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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RWORSES4%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T010044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIDgo91qejHIPa5Im7Zoyf7BprWEpkpiB7mQTUj4uqZ9PAiEA4rfsOBiu9ZE9V87M%2BHaturNs8NNOfHaDzTdokk91n9oq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDJoPVsYEbAn%2F37RAhircA2tp%2BjndzzvpYEV4aakZvx5eVtpC7x1QGBhA9udcUdXyW8J833J0jQ2UqPzj2USl8H40z0wAqNpZwgl1sG1VhW270q9DBPIXvG7kEbmKAJTW4FOD6TJck4Vnd3lAEKmiF8sQMKmV8h5uh8zgZUYjx9vGyanxixx1MU4FiswMoApca5WCCd99D9o4Nd9sTZql79ubmBMdVZF32L%2BXxSoFV85uBOUjKbyJicMhgbIGnhvoEk6t9Vio9OLiccAYxuJVrx%2FNEIdB%2BzVopihxzJHdKV8rZIEqPntbyKVyFG2v4lcOIHCEw5jvoUPZrmpwMqNqWjcv93F1rS26nK3uYjmhwLMPtXI%2FqL6K3UdTGey8M3mbD73Zg2ROQBCNwQNm6P%2F26X4FwgMKR6obYKe6ZdmJ5wmczZiMPXaQ1aVC3uAUSVNK%2BgS0YhzWPDkDhK%2Bbjz1tcVHbBn2ToaRs30eGdZ0xqtAiYn2Fa67jSZmC2F3DWiBDEJ3SoMwQqeFLxKZZgbY0WSZWQ%2BkpGwaaOaNigmGJch%2Bwo5u%2FnXUl6%2Fz6gttD54bz6I6rOX7NGTFsQYziZJBlRxg%2BPka8h5NSMCUPg70B4wbQfFLKf%2BrQ6zXwCkYRrbdsc16EIX5k8NpkOZs%2FMOTOmsgGOqUBb3dpnUdgN7Gl7fZ58b1RJ5aCDapxJs5dBUpwRQFHN0w%2Bq%2FM5DKlNWEic2gpncz33k8THLCMjGs3rkxml%2FONu5wBxQwaPtbYtSCeduxJCJ8fWxk4f38hHbLWNrtC6q0To4bEpMoz6z5wONDdBl%2BJ9lneVTL5FkkeDk0fgY3eeMMG2cowqUYExxUgzVNvI3QkYmnPIS8H2T5NSEL2gFFe3ydy6plBx&X-Amz-Signature=be4b10df19cd3fa1286f77fccb9eba65d23094b40b9f0ea6c11de7ec4c2d80ab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

