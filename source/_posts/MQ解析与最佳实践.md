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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VFKWQK3H%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T190041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJGMEQCIF0CJIByZ%2BVEYx9U4DiE3KaOrdZWQkFb2%2F6Nm47Zd3GIAiA80Sa9%2FIhPvuKc7O%2B6W6QNP7V8uzziRGZgU22juu6URiqIBAjL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM5shLJsFXJiGsCoafKtwDOqg2Esd7NSFLu8ebuYgsEdiDA2mzBgAvdcMP4%2BbKfz7yukTqWsTVKe46l9HR1Uy0TDOHNuP2o%2Br2bxjbXffvaeRldMn73QZf%2FSV1OISTcWhbtFnEt3qr7NB00TTH07CqpIcCKrxPD8iAnk%2FzfL2Tpr5ihNE5LrxMZcO5QXIhxmKa7PadSNmlQTxJjMLimUQPbdYM2nG7GREvRMNDmEiin2AXuGLIhBetHxg1O6RETfMPNFKhmyZ%2FtbCKE9afXVkAhNR0d%2FLgevjfHH9lbjPjkOaRc2SvUayVrDvsPju9bbxCl%2B%2BOvf%2FiEZloDCO%2FcPpbrvnkGQjhZpfZ2AA9Fumhhr4JuArDzIAg7pCg671Dtkm%2FkMDGoeMtccTZROp%2FB2cX881iS6o0yK4Ll628UwLVIo0FjbV5WybR5XERoh%2B7nR0yZ85M5amPQiqGAgrjQMe7O87IsPH%2F8QzNu8GLpK3BPSWrPlRfu3aHfIK6wCFNU%2FYaNNYkHhwGFwRaoqFodCFz9dtvLQ6L88s9P9ZDOKOvUtuimmSr9IoCUPY0hV8jSRMK51deNWrOBtVWx3ovcCfpZHtr1UQ6goOa9Y4cRXsP%2FqBGOedu46pDJnuljHFL4TssiF1sHhbFff%2FZYzUww%2BfyyAY6pgElHiPvry0I%2B3MX3l7fhyJZhM%2FbQZ3wWj1JYQ0%2FM58nMM9jZQBG6TwheC%2F0lTRjgrlKPNoWqvadN5j2USp3hjFCmMosJ0D4hgzYx8Qld1oJqCHdHliG8s1HmkpxGgsTN4%2FUkNh8ntvBGrmsCQdtzZSX9qC9OptHmFeKEP0vvob00v2%2F8UBqTDpDL41x3tARM3K%2BQbSBUAK5ewVgZ6r2LpdJEEBJQIon&X-Amz-Signature=958163cfcbc224e55857f119fd589882438bec0875af5953b5538cd602432359&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

