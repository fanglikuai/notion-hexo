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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664KUFMSMM%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T050055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD4HaM2J3hV%2BO2nLz4UAa7ULqi1kOJRBwKPGcPwO8HnIAIhAN3VxvG6Lzh%2BC51Mxvrm6I%2FMSncKPGonKaw3PvnIB%2BsrKv8DCG4QABoMNjM3NDIzMTgzODA1Igwn2EHTaXk3RFGi4eEq3AMaZXwZGECLuM5UHA8%2FLvqPG6mHPPh5FZqLBAbqREgw%2Ff2WaThSaMqnn9TO2ojEnsN7qd5RTBfePJFq%2FcPtOiAlfwA5wweY3I%2FGtN7Iu2V%2FcbiKYWliab4oA0ZxipRoSI6fS%2Bfj%2BuDicxiNvoo6nAniLIhggxxPrTpm85NXPSUxQnchkh8Dqh7f%2B2UZrr5HVyPkbBPu6rbA0n3%2Ba04bRZC9Oc6bbDMqMHJ57evzHyLlIpaJmrIifQtfcd8G%2FhciURua0wGD0ZWCQ%2BOq68oCwxk16ym0U0bYGFzcsvrbZqxer1PK%2FJzOJmvqRiK5XdtNgHhg8XTeVp294I6JhRoaGTHS9UkV4hW4jkX4q4XNhzJwqfI4uJsvEaAK3dSKpTOIoLbcqAwoOGLNWRXdxASEni3LZDTBxkc1qxGLDfidKlIzT158ODOh4YR9Y4DfSXnsQqdOcWqbjN1h1iN0jm0eA4EAAxGIfUVEo0r736d%2BzB6xv7Yh0E0pX8jf%2FtthcDlnwbjr0yeWlTT%2B8UZJCiNT7FOAyB5Pax2cJBhmYWhcilkA2M7hXTj%2BVrcYDgohhdYzIsAqGnZMdHpY14N8RcXTENjaNJKLKvLh%2F52UTXl4mgaOOyeJqDx01CBbz7kzGDCeiKbIBjqkAcBpiJizr0WJ%2FCY5Wsx3HcRpMcQ28NqxTPUTaO1O38N5I96S8pSohf23ksWRaVg6sizMAXjaFKRsJlYSduxZcQ72VyGgU9Ij8E7nzOhGcTnUHox1947r0HLj3Sya9oev0rhc5K5EGGIgOJvxU8vP837AEM0aGPCaiodGRq0rN5blDdSNk3mZZGqfG%2FmDu9YKts2pIIJ%2Byn6cNsyQ2rNphVgPm79k&X-Amz-Signature=51fd301343b52be85a0bbcbccad9bb41ab198e3ba5e2576a4366c0c971729dbd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

