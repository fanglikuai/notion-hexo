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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46627UDHBGU%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T160049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEE%2FIIY9gDHoxGUNTgROBvu4BdKZEeXcYq%2B%2FyPVLPW9VAiATCQsD9PjBTk%2F2VbO1O687byfJLnsA%2FcaSjYY9L6BShyr%2FAwgtEAAaDDYzNzQyMzE4MzgwNSIMYo6uPfeYjrQHMMs7KtwDE%2BIN4516J3tSdr7q3nqKmhyykNiLplufmTapmi7sTgr6l8MWE0iJ4%2Bt1nofaaGTI8m9YojnwUX%2BwkE2ut2NrwEEYsvLjWSY0woqhXdWe%2FWXFFTVIMq1Xlg9PbuXN16AUkVGNB9%2Bclvz%2F7J9defYvp4pF%2FA4AUqELiNefckQcotUv27cSESbpZKddEFQZdqq5kL2kYr%2BC6PIOLpgl6Gl1YYLRd54FMh0lkxk%2BHlJ42POFZ6o8tm7%2FuKvEabuhi%2F2DT%2BkIuXdg%2FdLpKxpQ4ZwxMLVUb4DLqol4XohQwkM56QC3ShTtKP1JfuwGAU6kI9BznIB6QJrZT5dF6Y%2BP9mqBWMc67Zl%2FOpjq%2FMTqjduMHdEmZmWQVUsjY63VeT2DwmyscGQjgwm%2FXcldl206ICLMmTmQ07uHGpQCZdXYqMA7yqvdFG4Hzs2X%2FCPwJ68ddiFA3PoDB0z1B0w7Ectljirz7FQMo%2BzlqW7VcTD0OdjguT0ijwgQ1P%2FDhFLXbgeBqMcA%2BD1uEQgrf8OBetYRRummVMczGpdmGshJBzPQMTBQcHy1V0dBcrM%2FqkppbjY1DDe3jOfjrq%2Bhgio1HTY0p4PTPJrHrvytf37yMnA1LshVqJZaB%2F0p1XVMEYbsz74w%2BLeuxwY6pgHRnDIOMbrClIhcefHjX9jlbg2B5kvBNWwfPGpQnA11JOVwN7n3PAZrEGD%2FP31a9q8BnnCdjZFG1R1v5yIqk3kD5uZmXrYwn%2B5tDlVzd0egRepYCJW%2F70ADabUZms8%2BLAERGxfNS3IlDW8B3qtLd1mHQsRPX7AOEGqrt4AVKyPfm%2BFmzxELS%2FeiG5hg%2Brqf7u5gRDNugfWZf5yf3ajBUAUP8Snl9Bpk&X-Amz-Signature=0eb2aef5f64a3c3d1b46014e46287e4844d2ded241999fab96719b4284031cb1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

