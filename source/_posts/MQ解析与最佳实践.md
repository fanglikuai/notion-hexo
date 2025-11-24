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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X6F7ROUX%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC1liHvDd8HCs9KqQvqSmFc0Ena2y2NENwQgKtN1bgapQIgeYYtNhWXfAMn%2B3NGoPJElwqWdBF2pG2QnKnIAhk44W4q%2FwMIXBAAGgw2Mzc0MjMxODM4MDUiDC7dy7348baSzsMkNSrcA7C9u7CkQ02h4zEzAbOG4AS4fVXJMyvSrtZUck4%2BMmjUsKVWPPStPfmV2SOl%2B4CMI85NyROlrAcB1dCuXI6Bo7TU24BIlkVNrzR0FUjNRzYwzCt61xOTgJlBtGPhrpszh7AjRFExF%2BJ32d7q%2Bb6OsOtkiLztqA4S4biLNlndJwQGdMNe%2FqxNoynyW%2FugWx36m%2F7eIV%2BfzexLKGeC537jqX6wv4CvDdktgpsP68xnOFFInm7C5Hxn4o%2B%2B92gBhQbjq1uF1EKbBHg1A3yzqsSaLk1cfgNQbyeb2QvLEsSQ%2Ba%2F%2FlMKRplg5k%2FamGuATJol%2BR3v4pMnkhmhQS%2FDQ0s0ZHJowl%2BySlPsBY5teRApD9F3x%2BKWyVFKoPFMI409VdLgEAE8DQFDJlrVd7ly7Ev7xpoen%2BUSC8%2FEwL%2FBDNzcDBBiWxb7yFgTTpHXUVqELOSwO2D8bwEar%2FAxzjlwDC8F7Balf%2BRjNEZueEmT8ucjE0cvoOP7%2BGkUEp3uD1yENDVbsIsar4reOcbFerga5fSKLS4SBo5lGq%2B9SyCIgQ6tfrFu9hM6u6T58UdW7HNgjp0BUksnF7NDAg8LUMM9hFjtUAghK52tTM2T2pS3tNjHAA4aXsLI1rE%2Fu7W%2FZ6NJHMP%2FXkskGOqUBV%2Ft7olwhvjU%2Fg%2BG8tL0ceKzHsbiBCIQqDzSZgAFKwCCNDtZQnhOtkLsxqyeYQ%2FaGqDk0UkXvdf5bFGzQ88ioA5s6r82OGTg378TLxj6A%2FLOuM1XOYnASpuQYdd9O1UMnGLA0GbgQinSPfAivR5F4dfSDzj8G2n57Uv5ItJXDx2mdgx%2Fi4pkc1EUCFVrvHyXpSYZuMDPC0lgEXLAtf7pXI0el5FW2&X-Amz-Signature=cbefdc36bff7d8dac76a659368fb395731a087462b6d02099d20392e1777f0b7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

