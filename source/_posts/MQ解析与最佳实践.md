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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDPAESQT%2F20260324%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260324T031821Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAvhpKcBF7n2%2FEyOXeYo%2BuVUWSWHOXsN0Fp1%2BdKfeiknAiB0%2Faq9wQ9hzBkxB4rmZzoeBe%2FLVrB3NvVM5lEuU4jCyiqIBAiM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMK3BWlkWP%2BBDcl8ysKtwDD3v8EPofFg4OcgodaIE7sU1mHqukQ%2FXeTpgtyxnVLyUce31DJAh9njGmPafcNbjOyygvbGcJzp4Vvwk0hJfop%2BNco9RVjppOPvFqX6MPd%2FdnMCzUFikp5wzSI55TYPzy9nGq%2Bht7KPlXhOormu8g1vNwUO38BVzBG4r8qJEu3xgMK3EOfxVahjRmwOGwxOBInU%2FhU%2BeScyJb0%2Bx%2BL4ernDnfviXMNWC3iqCDzoWNr7JknO1btKzBIHRRucAxDOnZR%2BL%2FocimVkVsfmVzFlnjdK9P05Nc6iLNc%2F%2F90jXjvKsSqNImiLXiWGig%2FViD6ZR2FCL9md1ypny6eGRbwXf58OQbAoNbJfPioFPJbmObpEItYef%2BvkG5xfP3ki7xq9E%2B8sURyEi7YgsSzhsgiljyZmTn5OwkHsYD54yFvD%2FyXyqqAPXHhySkr6Pk31JGQ42LRCjJLH5KOW6fzag5N3Bp4UQYmAjlX2ZlauARfh7BJdI11xtCegr1v3E9WMiKKOsEZGIPXpjZsM0QBywA%2Fj%2B%2BldMhFntEsZzqvhpvBIiIDQwWJKU3T7I%2B5QY4WaUSivuUoz5gJMCQ53lbpABn8y8CXEMF9VoDIYERYiKT6U8NTph8CRdI0DW%2FUusbvucw0oKIzgY6pgFhCo7Q%2FPDiA8pkAuaGVG8tglLstIxmaN2sR9d8VQT9Ch58e8rKLrO2lZUkWFyGRfxOiBAuiUnTXKxI1sedCJT9h41fzO0E0TfMkgNApS%2FS5zYjV5efJmbgozrAu1ZwGV3n1LtW3fh6wMtbrHT%2BJ3QQoAeuahiMEkD6usPGv56qHMhiZfp9ZUzhUohm%2FTEzE3uMq5aGyKV5ajtj2UqX%2FV%2B1RovwPd%2Bk&X-Amz-Signature=34b11dd1f0acc74ce4088c24bc4b5444253eced32582d8d48ae10358ac394870&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

