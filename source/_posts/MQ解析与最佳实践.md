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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z3XXVBYA%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T140057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFAYoWf%2BF09uUeGicoqnsxRf7dthGHF3pEmcdD2nJuu6AiB6n0FK%2FHN%2F6%2BOiTxxCkB%2Fvgx6w7S%2FnQsam1MsiJlWXTyr%2FAwgTEAAaDDYzNzQyMzE4MzgwNSIMVockYpFEOvER1csFKtwDjxSpf9SaVsCGvaD6j8oW0cBYKYNn%2BNbddjkWR%2F51tFyLKO8H15jiCkSb%2B5UfmOXtTedIr4hOM8ot1Z96wGNRxkMDuZvQxhYQoVZSYtTN%2FtimYU9V1ezSbHJwJ46MSl80MOzCzbK3rFe433mnp22BmxF1FOQhXdZ0rnjGH3UmnZqsVkPmS7RRrCrFN870e9uEwtbZYT7XQ1RQSVCqc68EcNoB82X1UnP%2B7ai2DQhJqDzkzSfk8mNqpqAtL6QsZr4zmhxBUOD%2F1ca2U3nCTcDnh2fZZ37lUr0xHkWQm2oq5XZhRtt%2FKe9yGsDYM%2F7cTYJiqILlp6MMdSxZde%2BC0EVL7lUCFoiWOugzAaO2bNyYZsB8uSIv5VSUkLp58RIBtc9SyyTLhrDI8x6inHRSmFxTw0I4t8WHG0U9vVfHRVDMdEL46NkQve2Bwlo9m73ltsOKvzTJk2clu2zVixjJVv%2FnbiHDHHCGQPuwOkKvLsCy2mb0yMRfFHP2CBx0tieXALohxm3IkbtYMJYIUBUzTuPrkJjB33oOyTtx%2Bp%2Be6dAqREfut%2Ff5VYRa5kpKaIaOgHlVokHQJ6l8n%2BEOMwa18W2Q7xFTxB0gEzjpJTexT2aLV4lzltSZwVjNdE%2B00eowsZ%2B%2FxgY6pgGPWRM3dht2b8w8ZIWhSHg%2F5E0VINGjOjdnjCGqRU6lLM2I1frZcbpwiAkf1GdBRLmnm4c53QypWJJGITZwjQf1rnlfcJ52x%2F1ulOJH4DPPe9n1f3pqqv3aQjL2PQWW9jrUYkgDD0RqfHBoVRYWM4iQVrCY%2FVlTLHbfsUlnFcZditG7M5wTU%2BQJ3O7iudh5KYtGuQdVr0uS03Nw1zbvQ8c73%2B%2BlXrEK&X-Amz-Signature=84064b9d3c4bc186e9093807d9a9ac0459c0d8b9c4e11352c9806dbe3a87f54b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

