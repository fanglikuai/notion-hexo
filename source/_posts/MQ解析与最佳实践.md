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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QRIRIOK2%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T140058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDlRayQNWihGHJkelA%2Fu6tSDNFhTsakkzY7faCZOIwd6AiEAgfGqIMilknSq35Fl7XjXecHg1t1wxqY4hawZ6AYUDNkqiAQIjv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNuauyf2y9UCNAWPeyrcAwSS6mj4RB9dkox6jmybUb5VgWmZmlrucuPFcwwkvK3HF1jxShNSQqmesHFInuQ0b0o0KW5P0w68LU708l8uIvapqMRWOiXLyA4f5F5NPTwdVqg6YvwQYoEi5NzVvqgWiWQXcSQiOIjawOvFY%2BkllcDB0qsZUxUHRF%2Fs2ccrdiOSEZu1qoYdoNTxpxQXeq9C7n1Shv5HFFJ6K%2Fj4abrZpO19sggAahXWQ21uQ5pdbBV%2F6yOyuQU7eYFSmWooYPhyyJWOXZukFqv90Wyt%2Bg5PaopJpiSKmQ14WtmJMw%2BM75SE3fr9SAdYYn7CMGo54JQh8HFUf7sr6DQDIz%2FcxWdz3TrRZzPdususqT2afQ82LwHaJZKnEFwRs2UDQZRoOiCXTUqs%2B8TMYNQy45OAeEHx4ThRlXs4t496kSdZWn3uvMNcKO18s%2FeIf52vYNmYFiENCy6g5iM05bqTANWZV%2F2h8GSvaA10gjKdofjt766teHnnezJlILvpjXDIVKbomz0dqyyX5oJaxwwPk1%2BeW5k0O0abIKGsf%2BKK1n5g6GUtikLfRvhb3BIvGYrxrlzwNtLiFaTHnWRQ3suQ%2Faxk3wGfN%2Bd8%2F9b4pnEs%2B%2BTMeOwWWkG0Lb7BiDykPLFSUtPvMOqTrcgGOqUB4ZQkytYTLEiPtE7SarXUti%2BFFj8Ti8YbN0Bsbzie201h1sFgEuJgTIGQsaZr49GMW6ZOkyOO39lGhs0bmKf59UakryghW3YcKHKBaW078QeiD3%2FBUidfFaEwWk6IV5kXlQS%2BwCGrbjFRdzXvCd9JCkx5bgPMNrQ3VchxrgRkQSRpoac1K5NnXRJVuZabSMPNreo3XwrKJezO078sGXGvZ%2BK%2FBZhG&X-Amz-Signature=7329fc7e8cd1f2786113776330f71b1fc8209e744a399c2852ee2b68e52c6d3c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

