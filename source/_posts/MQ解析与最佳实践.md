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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46653E7I6CA%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T100044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD1PsLzrufQ9Z%2FDTHyJopIFOCm3GfD5SzLa9zeUJ8P3TgIhAOM9zYZxu7ijjMacalftN7lhy3bIZkp1FVMpln9tJa1SKv8DCEIQABoMNjM3NDIzMTgzODA1IgwFuHThthz4cSF7C9cq3AOTORvbePFL%2FnCuS0IalEyzXgLTsriO1UNYlq3N4eNu6r2EQex36ehP4yWbUZ%2BM1BH7TOlSWQU%2Fj9Gg2cwxTMqJp6Et8sdjhprICiO605kpsbAefUHMwWSVSwfPSlNIRR5sJnckKx5AblxvZy1ZTsoo%2Bb2fXGYrJav6YjPeHrDj8gTwW2nCs09zzJHUsaWsDT0uRd6%2FkZZZ3SnnSVrGIHzfAQk%2B8QxdH2%2F%2BnJUEqzDpjYpCvYabpgm%2FSemNsUVFXBVWzKEZIXvdllBhlEnYmT%2BlQSIUZEGFRgLuKVdaq60lzflON6ve2H8T7Q7jrRJNRueheKwNzMMml7OsAxtyN7AcL2aD9oKOmVkQ64Hllvob%2BQOQ79eflwLwolo5Eqf5fcGa2nO7Tz25a2E8dANQCHyw%2BnUQxK8k1GoDleklNkj%2BLIRy%2FuT%2B988AY%2BF9OKbMqF7B9zeX70noIKdc8MsneeLONzvC4Lk60eWw6zzxQV6SX5HY4%2BLqzxUHbgOZRerNVJSaf6CaHxUOA4qA%2FI3HbEQB3piw3TLlxOJtXY7asO6k7aZ%2FAdd6wME0SBPaI0FGXtqw3H%2Fhl7lc%2BnksOWRCN91eMJKq4zRxeze3QAP2uCEktTlVBT3dj0wM2sAdIzDGgrPHBjqkAR7Eg0hTVwsUC3tOE2mx7mTEobaQAjgtNf2NVXBEE7QExA3zQxOKyOEuJ3CZU%2Bmc9JSqYDWng5AVfrUbo%2BDgFipZwhpiyIqaXWGGmHB3SACuuM1Dg0h6p%2BOgaggiTm1TnTV95Dt2tq9%2BSVs%2BSl5xUXzTyzYiWH43Qy%2B6FhPVsNpkVg3%2BerblNMs9TMVXQDXgtO8esh4d664I77Gc7v8BpvA8hGLs&X-Amz-Signature=50c4da3709da84815bb105e20dae9061f683e78e34c6c2614c7e5cecbabf5d8b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

