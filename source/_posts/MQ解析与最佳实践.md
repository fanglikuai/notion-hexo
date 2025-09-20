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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YUU5UFGX%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJIMEYCIQDBTZ5a5YjHxPc5CALr6lDoK2BpbOHN3%2BwhUfwXxDdkvAIhAPeX7GRkNYlej2f%2B6UkxSisOkUe6sT6gwTi6Efn6T6TYKogECOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwoBp9a34nvTqkIbNMq3APdUBeaexrHilB%2FMNxiB2w%2BRUWorS2zdH%2F3ImxATOXMbiEgGabXVE1c1cL%2FCpT3lr%2Fd04JNQU2e4JoRtmmcejeKBQNLjgqt%2Bc1CjpKm4%2B0qltW28Ej7pOuQxOO1hlzayjp0g3Hze%2FeQ0WWdMD2sQJdLIdKj%2BW5uH0Ni1eOtITs7XSetkcwm4AgZpGSycRyBpBRmWASbEI3swNwJd4M%2FD4YTU%2Fuud%2BL9ErAu5guD0s8yLkTk%2FN7yBHpMFrbnKnQoRjz6jvOCe%2FjdjrxKqO%2Fwt3eqra4Bxxh9cShdYN7gbKrtkWTqFYxIjnypO7d5FzIgKKUIDahsZPrhR4TybVxSCnWnJxscIifCLok3MLLKwMr7OQvCe%2BKzdaLWjx0%2BrBj6rm5rrZvvvG3zJmRjEmnMCQEQOaXMg288w0C%2Bx%2BeYyMLOuaMmObejRUtiQkB%2B5jRT%2FQMJ%2FYXacseYjvrD0wapGEUizhjn6gAmEt6lXPV9EJgu5qJu26kuMHMCD0NyQEpyCwUPKBROHohSsfPPePoSLMCYrW5SojYPW7Cfyk1HTIfEMPUqm6Q8Hw7CBExI2066fHN4g3eZk26QP3FfaTKu0CgDfhQlIwz4K%2F2vE4RK%2FuICVdRk3faY9Duc4OPuRDCv6rnGBjqkAbsMETn1apvkCWd1c7hSHofM86fZAkvjQjvC3%2BqVv653kjQXIMYRbIaseGmrKgcEsxnP1r1lScVwK1sISqTqn1flCrQOOsTG1NGREDmNAs4U5n5pQTaoJbGskR%2FwlQx%2B4Tm9u%2BEbXQAAEyk9bGlW7%2Fq%2BBr%2FsB7foCwOTakNrHLuuLbN%2FDTmzhoXGDZ2s%2FKifn4tFaVgmpAG3kQv7JI7xSeWabQAO&X-Amz-Signature=b0bbfc0cdbfdc7d8921c4fcc24e9f1e8840f3e3038a439bb6f4b85c55479f2a2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

