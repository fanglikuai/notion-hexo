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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZDDJ3MEJ%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T120041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDQaCXVzLXdlc3QtMiJGMEQCIGy7LYCTwggoD0KY7cF0wDd7%2BVgwzCqeVPCL9e8vynEdAiBRsD%2B7WS8P1xzWroY66i0Rqu79qIloH8UCKz8q%2F1I1iCqIBAi9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMFhlv6EtaK3%2FDoCKOKtwD%2BxNgO2%2BZxwUm8ai5uvj14jSVT8S8ugelVCbveJj1DjMiX9TNiPzHkBuzai6ILHUxb7Um8JmAJgf6FHCDs2Khd9RW3J%2FFLpP6nndwQHe9CAkeM5OAi7McudXDwPVMQs%2Fz%2B46KSpeHAVU%2BPceX36TRm4Qt9f7mqjH55KeJql99slF2ogrplqEH4XMdJFVQ0u7WXg5y%2FKQMCDAhvpG9GWe2v5cXMgJyqD7SmS6eoje5gRIZNnxmKKTOC%2FkUkOWHx7HXp%2FM6eQebAKFYdP4Ilqg2AyFPGEnaCELUGEHSCvMnQCPcNTj%2Flx3ub1r1GOIArUe6HT%2BjWZ%2Bfw3uvISl5gIE6aJcFmweycPrbNcS10e83%2BVkULkGXzKFUUDTgkQEjIE%2B6RqX4PvgiGAqOJV1WzNkm9OL88iGiwdbvWVU3uAI7bIK6Hc0XbSIkxnCdiTKMljHhXxEEJo5jeWlrIGo5EeTy9exuwld7rEiSoC6UyhafrZY8p5mpK5gR9Vba2DqO4UYPWZyptgkFBgpeq2gYXRYpv7x9nabpyGAJUk42dF8TPNmLtHyWKhlbId%2FL5Imt4uK0EpYPC7geSmbAHzijTzS6e0XNiYZ3eHwV1WnH0M%2Bo0P8G4hRbiGUuDz02X98wwr%2FkxgY6pgFgvsO7B3sbN%2Fn09SgcYSQBZV96H4GMVqTDmGMtmeb6eJQ4MAlYluTf4nBgI0xSDsBPGM8Kdtk%2BwxKaMPfHP0biHTBAEnQKlqnnH28Irys1kHxRDc9HJAYvn9ZrWQnXnK8C4NwenBE0BQ%2FOexj0PdI6tLB1kwzupQZgivGtsmKVIUE5gLKAE8A%2F3WlQO02uS9oZADHZhophp5DABKQdWTHUySwbyONF&X-Amz-Signature=45dccef909ae3510452e6eddb3aff04ff03650f71b9f6d5bd933b5876741f61f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

