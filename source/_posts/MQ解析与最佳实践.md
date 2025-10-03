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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T4UUP72M%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T070050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC73I9fZX2sheGlrPtPMCUlgMo6%2BYQ%2BW8qXKFjlGFMVjQIhAJpzG2zeIeVnktlTLjvm833cgDX2MPoJBK6PfjNihdVOKv8DCD4QABoMNjM3NDIzMTgzODA1IgwQLmCbXtnRpJfDXMQq3AM4%2B4V77jgTtnQvQZZbnuVC1HdA9lmMBzyZFKy1fhZitb4zbclsCc6uNyFk6X51cHF98tyEzTfhqEsBNsl2GZdZ5DT22HpmS5bjrCL8nAZFiqHkwtgxqdXv8c7ic2rcWWfrlwdoyS%2BhVHbgAGdosJ29wgxkDb4rslgWlkNtb61C6HPnIZlIH9dPNI7IIjNcfNHGDnteCln6qsNLgKkZOhpuLurqY6sqFYspo%2Fm5ajljn%2BJVSIq7nVKQQ7tebPQjLx4cl6IY4%2FrJ2lOSLnGfwdDHeyxHoQdaLtjvv537j4RXPfDpQop5NwrsPkw6Hfu3NOSSa2o9PAJw72IAxAFy8sFsREJ49tWBpmaXEQO3Tu1Z6Heewfu2%2F2ReP8KY62cgVr4pDkGjiRHD5R9OsjtSDMMLMn%2FoJgK5EjAl4SqMxv8ynQOWwxJu5wd8rI0zm9p9RC1DUNpU6FIQHbC44TKZ%2BcP9lKMXgMyzPk1Gy%2FGUkgwglLSY7z6YKKeXYFHMZKl1uoUcwIXPRNOucF7lcDSl0uVk2jZIVdrip6EQLBUpQxGTbND8rqN2XPRe9gyqUuHRKCi00QclKvrMUgLJcWfgPSJ8DJeljiPYOljgnx7WaoCltApB3U82K2RV8jnQGTDXq%2F3GBjqkAeTVtQSX7Rs0vKQzfDfWRwhYUCTN4suP4zzDEfijVxgbEcK6bQ9JrKXHEczVEtn2%2BJoeScw%2BCjl9619gxL8%2BpmS70VBs4aDF9sx0Hpq91kv0K4uPIgwy6FjG%2BmI1wEoyiK7MMxGo4oyitl7xLLZOVzfg%2BBLvaO4Fr8mJDMv7P963rlxKjh%2B5y11tRf1fVYqiBxHnag5j57onGBqRKkEty0KsBk%2Fg&X-Amz-Signature=dcb40b340d9568e1976d728fd92b3bb2b32325a418c7e72e032c4bd0eb6000da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

