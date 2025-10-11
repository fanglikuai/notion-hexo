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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666SH5FXBX%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJHMEUCIGyF8wiIgtP2eXTv8AR6KqLyMXTH11%2FXTowe8AQTVQeSAiEAotazN1zaHHB4aK75OplWE9XnMG9xE6VnyBI33Cuji9kq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDLomcy1EAL%2BsURKJlCrcA5kVtXhYIFFOpcnzzdV0%2Fbi9M8EDdAztKLpgBKTZRk6nFt5reb3HK5V5HqG78THWK8oIrr9Sz9LtynsR5TLO29UDWXkeb9QlC8IzRYCuTuIUFL6uFGHPkSaGdX%2BOind0AMKA%2FB2IvycVlz1VVgMxmDiuVwuIXSARAR1H6rn3jULzejcgY%2FTST7xBz%2FRYw3cxkc3%2B0PmWIaLnnje8PQXOKLwfGbF6iOg7S7X4AK464QirdSrXnO9GwbKfSUt0x6ekpDI6Vsej6O99tw8yu7QgO1MN%2Bw%2FgSoRNoZSVKP2w2Gb1NH%2Bp8P29ZV6EI56QbEK6K3mVp484jHH5W7iOEJlGjrKD2bLDQC%2Bx494zktOIH4PI%2F4NGoEhJMVSOkS4nPgIuj3ylT2lOYsZlZU46iBWk3PZ65OV8M%2FSxKcRDQufud%2FSdNCK4cmL%2FwuplZFJo1zHtkMuvKNzJIBj8mxxXgxA6fT6sFwv81VofOt%2Flzm8XBdE8kN9GeIhUQDISuLTmnlilMn1xKRbxld1XdNV9QCcfeUSSWJYm8Kp2LZLcxsAUjEQe1ygtL%2B4Fk5zQRjqOPAIyHLFNnsX1oWbfmGf66BHG%2BjjkM0TM%2BspUgQCpqQjYVWhjDzup4ymkbDC3ikAiMN7JqscGOqUB2uSNB2UQEYyMpiC7QFbDY%2Bol4qzWbZnq24QCtn9PSdXX0GkE3OdfP8xXXBTkT4eVLbMVi3XmH1qrJDNjWg7umJiz1jOtomO7%2FBGGbmrfD01iVYHD66FpiZSSLz6WCGMW2TvypUrHX1HwB6hgvZYNjB%2FuzUGPGBS8NzZQTxjHXTbbssRVzN250AXCGP4P3%2F1rn2GiEOJdYFxoGQ5yiFKozE1Ey3so&X-Amz-Signature=730b213d89e049aef0ad892898f12d71e909d9cb783ee0dfab271c60c1e50574&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

