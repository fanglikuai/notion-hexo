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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666X2IJH7U%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIArbjS1RJv7O8bSL7WqYWgoi7PxapY7Hlkv9SAg35PEdAiEAgm8fI%2FOuwC6RcIRw9jAhs3VXu4Q5BDhQGXfWEsWsnbkq%2FwMIVBAAGgw2Mzc0MjMxODM4MDUiDA46e%2FMcqW9GZ%2FCaLircA5PoQVn8JyXIHZev9bjW8D6K63lULLorOrBubxx90R4HHyuEwzLtXgmHNNPXROsRYovcJLe87OO67aT0xes%2BmZHOzt1KDPiyLKe0UJK3PtfDoqCFsh%2BdcfOwhU%2B00eYqOf1p%2BSVmwOuZ8f5AumGK65oJNnuIDhOtSIm1boxKLJ%2BI1JBa6REemZu2KqJElIc4JA2CGtc%2FHVwHmrAx7xUy8r8ih5UQhT%2FfNb0WWYuZqsclkwYIzXmYxSVlGxCaur9NSmMK%2Bpgw1%2FYh%2FuVaMmzJKnKPtF8jxwNIwAXXGgSD%2F2JF%2BM6MyhZVYE0QCGcNs8V8EsEGHHG0QqIsIEGAdto1sB%2BkMVxRLAoTJ02CIMCc8vjmy%2B0OOIooSSAkbOiYKBe1nqFQgre4d9cSq9tzLDnJ2o2EXmmnlabYFr2zlZNVTlNTydrTgTid0kVNyIZoD2hpGOrGvmeZE8tObql5iWGeJNWZTPf2ZrTRtY66WBJGIq4hukBbL46KLQA%2Fpbj4jfAPlPYJ%2FHUBiE3vG2kpEetvvoqjjeqvByg9C8a5etnT%2BSizfphG2ob6vE9PASHSHl6lLga8h812675iygydMo%2Bu1VmCReDmb98IeHSTElpBLkXp28C%2Fn%2BAYnqqbEpCKMODQ2MgGOqUBYNk3d3NQxkOYEPAmDMRR5wSNCy21ahLlnMMR3vXl%2F5CNGr0XtS2SoPF6J04P0pXUtZ3wjICOdBvgetxh2o%2BtaO5Rg3jFacsW461WC%2BLTojV6L0EuPzlf0m4O4TX8gLX1M5lqGoMuTbEsaZYSvY%2BqJ%2FHxNW%2FJb%2B8ofmbWB28eQ6omKUF6R%2Byo98AKpIooBq%2BAWuO6%2Bi%2BZxs6mxfd%2B4nQsJGhF3QoT&X-Amz-Signature=d7aa5910197bca1ca53edbd547d5dc0ea701a55d7a27269d405fd2c6db0f72f9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

