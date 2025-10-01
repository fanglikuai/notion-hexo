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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WMHKOTDD%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T150050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJGMEQCIDDWDHpLZD06skkISv6k%2BlWq3vzKZ6V3xCzjdR%2BmXmC0AiANb3%2Bdgz1mjVy3Gj9lxg72UruoDjFqmnB0ECWE34Edpyr%2FAwgYEAAaDDYzNzQyMzE4MzgwNSIM3lrks6wzOAjhNG7KKtwDkM%2F24IVkGtSWztg1BGv%2FlQawgWTTGZw3%2BhhotrXLkF%2B%2FsK1FeaG6hcFmQNNpyE%2BXRm16P2t04t6Gk9TLmprlrINPHcYjFwv5akZgO8MEIBZKNoANM3PpSbFvY5soAEoSXlCkuQeHLc3XIO9HPeBpe5wGiS%2Flkn39m21XcQ6gnMw1zpYanB3g2h7Ad6YDyRPLKUg5Tvqoor6nSmTUfDWkKc3T3QwqL9Mx3TrOShseBf%2B%2BLapGbYaepN7VjehGcPlihLhB0fshmz%2FifOPsLF6LaXKEf%2FObjvdLMtsXbc5Cst%2FcbhrYmP0eDkzbUYJtvzrryTjElo4yF7LBCQBgsDlru7bqaQVbzmuZH1iDU7J6l0%2FNKOqsxGSlk6vt5Sx0vzpw%2B0d9YDUBMtrDecyA2KZk8GOsU5hEg8t2J3PpIM2z2tlLUlyqUt56m%2BFXe41nfLoxv02Y%2F1j1kfu2aGqdWWSDTbrymCR1opd5n%2B6Qwty9rPiWTb6cEVUlY6Q%2B0OYsjJp9jBXkw9ZKBaXmxU2S9wVmjwSVRst9tbtT3J4htQPx34Z7ESCwcxxvBhhzmIH5%2FLyC%2FVVTMsPpeo2%2F7%2Fx9l6QiU2xiSWKAyq3IjMIo8aVNngJEPGbYeburMFZI85sw9%2F%2F0xgY6pgEnjxiVyukB4Lsuc12tZRNpIIC9ZASUe4CaNid%2BclOkBoqfLM%2F4CoEs3whGd3%2BqBR%2Ftx635AdVpmH2kxoQ6QXR2%2F53gsbK98FybrOgHeD%2BRB7IWhVrwGyABZ1KoF4%2BbNjwK5O%2FZw5bdzyZ2HybXnLB6M6EqVYrF62M5LiebquHfsD9YGjFGaib69Ul2BMZSSWPm9G16W%2BuNhCsxn57KBThE4R%2B9c%2Fk1&X-Amz-Signature=0b21b2144f2ac5eb51ffc31d186aabc3eb5369707cffad0dd173d36de9d4844b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

