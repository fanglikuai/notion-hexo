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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WQXOBBEP%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEbzzkzRmZ6B7JBZNRYFZvpgYAIG%2FMVWfpLGGgO0V7wfAiAKoLb7FSQBmGH54CWCWLXHerdMKoZUJjjBv9tFD5ejVSr%2FAwhtEAAaDDYzNzQyMzE4MzgwNSIMWtjThEPcLznwdC0JKtwDB8VmQ0AO%2F3NkHId4aEDii0aYKXuqMLcpbmigduWwk5SeK2Yd22zNOUbQV9FvlgVPnzU8l%2BpaaKjcLo3L5J4vNDCB6Qrv5Gm0j0FYqfUbKd3vHlNH3H5QRd3Vh7DrjLLzeA0LPMJwy8CJeIYtfx1WSiWOkwpyvvbRkoWLXt27q9F7hJKIIEk8rWhKaLHhVtvzgVQRrfPoU0KSQWLFlYZD33VcqnnWjQ9Lg%2BJaW9tVzLZsr2cZNjP8WCc72SXyjytruLu0wVkbzAPHeeBOIDa4mfSl4FrR6W%2B%2BIT0iWDWkv8Ug2zBye5fxKsW%2BsL8dW%2F5XP%2BuH%2FPwXWbbUK%2FUOeD6yFi7G9B%2BHCV7cOf%2FlhKHCxbbjbEaiH06SV31PQNzfTWi6Mcdpiv16nAE%2BBhrEPo%2BKVxDpFgw1gqQaKh7vZZ0xVWotdyot3WMnhbG8rTUr33QKUiG1HsTfG2UC1EUdKyj0%2BtXSTIXhzXbTRSNM%2B9Q%2FSBna8RjGbEASbSFPNafVQXf1b6veyib6AbA6UODUR%2BX43qT4poEXlVu6NSKto43zg3fWi7SOPGThmonmhd%2FlSmv6Z5%2FX%2F5scKUDoPTFw9HkQseUuUElnG4ja%2FhLEaR1R%2B3SQCy%2FAoNX5Vz1gkXMw4JHeyAY6pgFGXoLBEjNt%2BmThY%2BzTW3YRB1hvBUL4A%2F6iJEL5AifBKEkOIASTOhas0uQXJ3Rz8pGumVE8eRjahSxf4NJweuiL5OD6BwR6JGI%2FWg7ytWq8ARxLjN8RwKSCTK2Gio75iNHi5D6OZwq%2FyKjZHASSxZinwNDJcTMz5Gh0ytNSYgm1Umo2vuw%2Bo85Ju0QUtyeHVrqHryIp%2Bh318ulVNEqPq77HJYty6rzJ&X-Amz-Signature=480a9d16db253150ffb78a01bdcaccb86fabb6d350a4b137764e2e8cfe1f7f75&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

