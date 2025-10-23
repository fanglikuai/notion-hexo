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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666B7V7AGV%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T180046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIE3VWj2rZ5EuSch0tJg9IJ9QIFXORxlHjSM1xhmS%2BH2FAiEAlRfPivtJv7nml4IDSZP8TAFbVqKxVfSf9tWJEMw0sB4q%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDHa76j%2F8HDL28Q7VDCrcAxTbl43hv9kOJtn5%2BcCWku4n7lfAkEeAOmLjfu0JUCobRQYF9fH1yPPdXRYYv08QJNK55Kl0TrOSK9Wr4KRt7gDeADliv%2F9LxIv2C9XHEw36Qg2os3MewCGoxlj2ZTap5ppPOnEpK7GiAxqTzSJKRfXtoPqqbKfOS5pynp6BMiyMyY%2FEwupl1QRVNgg4QJiBsHnEgcD4i3cyohiMYHN3rkOpWUdMNyH3F9ywWV7hlp%2F12sdrUePtnlaW%2ByVqm6JadqyinD68g4A01YT1ANeLFdZZWSTJLE9SctU12xjXdyJmXRHRB3JBKaaP%2Bd5wylMt4iAi2u5q7Xp%2FCDmNmalN%2FQ3GM1FbOHpTV8CKF5lKGpt8UxGYdPgES0dmedIsazHG6NjfR5XSrtcsemyNxgg3YVvcSJju1myCFqBQY29aIa%2BEs87yXoW5VPX2%2B6eHPX%2FowYflCpe0AFxIUnJxmVLjp0bcq3F39M32jZkbUpGi6%2FmzcrBFULb5vEhXRmQAUMHoOmfEmXMJUkEJK5HEcWr9k4fmuGY3JZVwZ79JSLPLkQA9Kz5XtfZWJ2ZCDFIiYQKv8brOuOJ9t68W8wSPPWWIhwFokt14YWcYHqqjfGgoRcQzH2Nq0%2BWTjc6DJx5uMIy%2F6ccGOqUBaKpl%2FprCsVA86vI7PSWq6Movis%2FiFdZCuZ113AlcDjbauEE3nqpsr0EvkJfgUZeEdJy79Jhi8ksTkvYp%2B4Th7wvSgmmFFQCie18azJaJ1x%2FT8sSu%2FaUFRDI5WqD6AjfVLePYU00f7XMzvs8SFjndh15mZrHmrAr3L9K8YBsWbAcwqBibB%2BNAL4pBV9ZjAJaNHdaJAS89Mcrp09ZRRJqBmrhVc339&X-Amz-Signature=5ee0957b0e10ae3962418455c711693b1ba0191a69b697b35e0f5ffa764b058f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

