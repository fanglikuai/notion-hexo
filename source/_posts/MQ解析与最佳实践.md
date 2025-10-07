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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YYUS3WBG%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T010044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJGMEQCIGHviEQsEu3xdD56ZD4oF45UCP7W147uowOSXJf%2BecIpAiAIBLot3NxQzmLfbdcqszNaUimEmZUe5Tn738u61mjaiyqIBAiZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMWEL7PkCy3eNkCWN0KtwD3LVYE0S1ZgdF8W2CmjYJQA1lRqX%2BHRx6p4C9Je4iAwaS2Y875N6aoUxcMkAVnx4i%2FxxmLdeZzsdjaqwFI6btJ7XwTOIUlRTG4QGMdchkagieAerVI9vFZ%2FLBmd%2ByD7j2G3oZPeyjEE%2BwGtbXD0HmcOuIdCq8V1hy0hTI8kBUXtqhegLzSoraBUZ81hCJvkGnKvj7FAjEzIqCbIDyM33vOh2iOsZLWE0ECu0IRi4O5upmNNL1HPNBYiL%2FuWLHjIvRQ6QoVK7BYLHB3NjE4mQ%2Fo0AflpQyuTpqwtw0Y4Oe71OGbHs6kld%2Fi6b53QZfvWmAR3fVtk86ELH09bhUE8BZfc4wkD6ZLYONCnOoM762BA50%2Fl%2FmsQ5sDVHFXNhFYnbLDp4Y774uIRF8Q%2B3Bl2sJbP4is5elLCPQzel2IY1J88%2FH6lFwzOPwIAteHiKl18rRpnKxIMgeit0tUULZwz9KLybf%2FQzGxcSi5167Nz9z%2F579glMtBiunoUTbC2LgmEoreEojzBZjQnzTLBXkXcbOg9QNf6LbWyo1DQ3YrZ8WhA5x96%2F3%2F9T4eQpEwepcLnERZcKEY0%2BXM3N7i66DvXAGXeF0ofW4qkwLJYZNTIePo%2FhBi4UKXBZWaLo5Z80w9rSRxwY6pgF4cCIMIzw5ke4fjOCQLTC5hLb%2FdEDlj4gQQ9CieO6PzzuceSKfeQqR9XvbNP2IL%2FvDL1EHCj0dLi1aQR2essSy3CA0hLgYKW168wjwYyAnwjQL%2FirnPQiwhKzxwDSvWvbURiQtBbXU7yuyRHDroXbNQGPx8axUXBlqEW8vreh6WRAqJuH6Wosr9wFM1%2Fcg8fanH%2F%2FZtNcxA1H5sllRgQnk7PsKkv3f&X-Amz-Signature=5e3fe16f44160c5e070f48f1466a7e91ffd87fedb31b0f60271966f2e96d82f6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

