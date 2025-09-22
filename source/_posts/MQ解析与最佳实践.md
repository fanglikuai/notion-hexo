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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RD2LLHZ2%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T070046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDKGrLRalaOc3tj9ioh30c556tIHZfMV1812iOR7tkFOAiBkL2dtPj1ZHXi1JyYJiKhYSLdW1hYqMRbJSWxgYOZ4nyr%2FAwgnEAAaDDYzNzQyMzE4MzgwNSIMY1GrHbpPh%2Fv7bDppKtwDuiOZrVqRiiAFvwimDaFGUGOsgCCsvFw7amCO6A7OserGIlE4dKXYEctYaxxG8vcx%2Bj7nPybglmkBBW63LSUeNRzh%2BshiGkYtn7mRTKqgFvRX60335ggwIHzMbGbEFecQ4NEgXdlkG6w%2FAE9o2g0crpx5ShAhfs94bVAB4fDKwMS3tQz1N39Sj3ie7rbqifjmx5kUlx52G05RYuN59uWgvf45Zvs3nylYJOiFhU5qDKa%2B0CwWOkkTa4jhK7th1M%2BpuyEOvSCBxQNWv5XbjPe4mVaOaFWdchzpogCg0AiUxE8G0YK8KfrVoaYU03Y8kHulHPC8%2Buo2n3OrOSR7D%2B5My8BNe7Ml1cFyBlNtlFkcbEuB%2B5hbu6KGzyNBRRE3s2m6wxv%2FGqdYKVEXYwOR6IEG9fPRsZHK0DLzB5SjYGIFmuaZ8JimuMsQFtqxNQAzDl5jY1hcOPhb0eXxvvy3HBfMMlZtO21HnET4ZDJj9kOKCC0wChKzkAgHnFYVIitlnpGPC%2BCycrgCEpODIecX21X7Vgx5hxjGzbtmpEvvBXKxWNC7k%2Fn9kNxoLLhGIKd0ui29plH10w8qE%2By6igPHeEW5H%2Bpyi54AsFxfLdKQZeoR0vmf1Rs1arBbHqcb%2FxIwn8rDxgY6pgEj%2BAwgZ5S4%2BIOaetcomcH9jqfgOeh1S2nNVVmEBXQgQtDfpBIr%2F2XBtY5PctZ7N3S7fz16jzkI33czR2MRNP0fx8PWrRrhgsnwIoUx18EzgBNMz%2FYSKyX29MefJ2WK2%2Bn6TDXbLczMy2pgBr75oVAus%2BAzjVThGTdT2YKRUty7V38y0JLzcprBCiMRJKUvaaBO6at1tNDH%2B4dwx3xt1LLNZ36OxLL1&X-Amz-Signature=79159400a32b8a6ca0a301836969250bc624fe9d2fd9b407e4599944d66686bb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

