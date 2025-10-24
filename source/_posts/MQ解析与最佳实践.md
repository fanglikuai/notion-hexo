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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T7SDWVPG%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T190048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDBDWhTGP6Ta9BzPHM6os%2B68hIlicrUzdejbspVEklYFQIhAPFDPORkuv4X4tmb8Os3uaJREtPAzG270JJlyx18bt2vKv8DCGQQABoMNjM3NDIzMTgzODA1Igz3mPa2%2F63lflnBFgMq3ANcK7peP0YphybJZg66uV3fqI6FpuKoaZ9ZQo6dxuyS9oYa4mSe%2FgZz04bwU%2BOECztQUbjCf6OWroyBZBxO30EUYSQFoyM22mucD4sVxmzH7SRzsngI3kZZ0PbTOnfMbTYctuJa6vazP6ccnDvUVeElj%2F74XLTjiZ1QIiuWo7pXj1mEaOokY8b2yss0iSpwAST7QO%2B37Wp%2F%2BN6%2FEYUxPbbgjK8BENnEU3utr2wjGFDpE5kec50VtFPvC82PbAYh8b3XAm9Na2oV%2F0bw9tGq49uxI%2F8yrk15f%2B%2B7bgmlExwINIbUgnzgxJU7zYlTGAiObhLtFBQlfI8n2RDGAAIAmKbDM1Qfhz03kAqMSLouY0aUwBLeXQd8ZqmmF5cRp72zQFPVpUqMwraEfQqbhr97sexqZocVFS%2Fu3d6YLkY%2Fkq0zZLAiVeG6bKHeeekuxTylu4L8ovSIp%2BWosbFemi36g4hHEPUJUp8kT7crsvvPpha1NuDwErRh7IXjTwyfpH9SPU1KWLVRHEtsYs5XCF9jITZBpJHHBwkm4wjswGqbhDKuIUihpUEgnlAa7LQQApzhuaCsWdymlebOizIm7lqkyVNp3o%2BpEYEW5rGI%2Bw%2B5kKYPpCjaLgDildmuC4veVzCEke%2FHBjqkAUOLhLxeRn7t2GSbMvPd7%2FlZYyTTzsLdu73CUoLSDAUvnlvuIhXXp9nR4vFijow3YUJ%2F8iVHONopln44kqF4J3a1FYkfmZBgRR375%2FK2hOaYxy3uYbqcZsAMlrf%2FsrSppRO4ewY2KNhHep6OmqYhduNkU12tOeNto95bRL0ANM7Ppq%2FL0s8fTX4yfudA0KpdAdnyqOwfCpHw3GpdmSX1B6mRpT%2Bi&X-Amz-Signature=a120053ba4f93cf2385e47d160d04e1336102b4752cd14c086150b5ded1f40fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

