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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R6V2MB4X%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T020043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJIMEYCIQD74PDqxyw1Wqcu%2BJS6TJZxXOCdxo6PdJjtosicj6KnfwIhAJpv6Y9IzQei1tZu%2BHKjnf74wJQ0Rt%2FT1BBCxZohQTwKKv8DCCMQABoMNjM3NDIzMTgzODA1IgwwAi6dUdnc%2BShoKKkq3AP1lM7OddZGKI15M6yNJvuuMnOEZTpz7Q5VB%2BEG96yqAYNWtIQdXmsg8og8QuGwIEI5fR0Im0Cczrjx0%2BN0aVoNrTXPuVYU6rnUiw2%2B7FR33eDD%2BTOhnsIgxt9wd0xZTICaqWgCR0LmM4mTHv6r2HAUPZjazKLusJkUwywEM0rZ14WYhYN7rhwMGIR6ken7l7XAnEQF%2BPpc2thTuXbD%2FcrLXPzQTgBH59RPirExcVqWROMdXXnNeBcHqtoF9Z1tbAT61NAC64yseYVrLGqssJC07XL3zJxK%2B3rrGt7iAJBzhRZjMKFhAbBJMvXs%2BKHla3EaxdtspJK%2FusPJekRriHeCHLbgWl8StKTWi3Tcz3fJo6AYgXI%2BC4nRQz3yePGrcZbOTe5gCnEhNpIjkwsh5T3f5zXO1qzVGzlIoc9FE%2FPwAyYfAdmq0%2FjCGvJLbLOmZ2ESvWLU1py6sCWN1EkRFhjuFDw%2BsdjOKozPgrJCb7FVeihKjgFJNrIhImsxIfQKqGVh1KvWdMbfqV%2BGS6xRI%2FRmzkEMeMLZjzFi0JtbHw%2Fm%2FGK1tVGPoSculGHTHmbHmBa2kAo9pwEGgi5fpd%2F4jGihrUyKwRkNKbfa078F1as3R7llN4lt57%2FpNmwLTzDw6ODHBjqkAUcWVV9skk8vEq%2BPWe8OtT0zTqedb0ITXo5dh64%2Bphc7YS9DpoTvkpBTyl6hNbTGeYWIPtzwNPSp32twn2xzO1gO8DzeSUzy%2BSnPAVPBYfM5VJ10blYVixBL%2FRlcgnqKXgImXeC%2FtUbL6P1p7MumsHgrBH%2B7mOkP56TfTONsvSb3IcViBN1R%2BEZ8C5YzPE7dJtJXgJHfOlbrVqxuVu86%2B49Q62pB&X-Amz-Signature=b179167647c1280230dbbc2c82e879598ae6442b2f42483793a6dd899403ad6c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

