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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667DYZ54ZF%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T230039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC6HpvpTbC4feVVWBaQp%2F%2FZVfcerMTPrPt2k%2BSj25U1eQIgH7tx4rikaHVsxeOTdn6wGX0KgvcH0bzObxppNaQEBLQq%2FwMIIBAAGgw2Mzc0MjMxODM4MDUiDOTPMBZoYYjkl2z0XircA1VHkX92jL%2FZRk1MgmKgf2sFrI4uF10X88z3smni%2FMW%2BO%2FARH7D7XrLsdoRvPd02oN1QWz5WXLrNMVSEj6Q5MCI%2BIEa%2BHGDZx3WYHQYHRoX5C3dRRx3ejJ8vj9ygDnh9W6cnqEyVBJUUxVGfhw9IeBGTT%2FlGbME%2F8D1hAJ1jGlac4Xb5WvSBjfeUc%2FIlhRAX49vhH7JUSvprj1aLLCFNosIiMffU%2BnO07YaabBQFcnr104Jqe4CPKiXOdUdp2kRjNz8QibhhwrLW75ktioY2xxUgDNbshWvvtdEFnyONIDLXUl1R72K1uRDG4PqmlgdB1s%2BULvaa84K%2Be1cFquAKNpgyH5sUj5U%2Fb4LekbArSjAKtxjgKhajwNz9LvO3trJMockEbV2myuUrrZJEMz%2BkuUzrMFwK6sTw5lov8KDPdvl5Ak6EVlQ%2B2tr%2BAOdeZTol5ykBH694%2FUzVP15wi3Sevv6yVBsVHUXq9ui9WH189Btke2HjLN%2B%2BoLGdZcT9RxRfbLEyfRu%2B275n9WdoHBdQocoGCl%2FjOl7kQEeqneBTz6TxmG1qiuLSUAFH%2FtjQIhRMs5R8qi0odACuX0sWQEOHI7UOvxADewmMfOWXmRXrGSPZAuf7eSUi4%2F8ETTIxMKbV9sYGOqUBdsuhfwU8P8YF7HvO6N%2BtUXhg1Zr7OQtJ543j23hsnSwOkzfbNZR%2FQn2eRrKP0LTPpDYF8%2FDtQYmzMJvRGAWEbbRARvujX%2BfOj%2Fj6BsBeLBm6bXD7Hb2ZUlQbnGhXkDAasW%2BaEUkPELTZOHD5lfdpYVIB%2F8gkLCsfOzp6cjn7lPOJGLlyANdM1pCHKPFFNw1usmVx8SNqpHIv9f%2BkBM0usyvYkF9k&X-Amz-Signature=c3756bd1adea77f21b18d47a7c6d215e362d5177c9eed77cbe9a218cb28a84d4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

