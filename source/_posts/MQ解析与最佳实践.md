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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UDWQBZK4%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T120053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJGMEQCIGsxXTEnxF4atQMxy4MiBnPBwk9EYaYgxFGovmfa%2Ft37AiB2%2FoaKvM4cq4%2B4xWqwFtQpnQk%2BmMzIMLTKe1FJCjg5bCqIBAjr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMyUkThqFB1wlg6HOrKtwD12H%2BM2rq9GGQqk4gvBUzhs1gPH0K7RPd4MIgCoDVglUsJ%2Bwa0q8AWawbcU%2B0B7nzsqfLIaS49hHrHMFOcRzac0ofmhxej4jFRkekacKSt5u2wUdnR%2FMgORBpbkju%2F8hpF22GtZVX6ikTZhbxAVGaUvosOAdLicbA609hB89yESfR53cWVgN1fSSXuMf3bbeocw7XCWFrcq%2FXnzYMGxgHhP7T%2FxjZSe5uhWYbU8qXMJr3XjLPobtOprogVy94yTm2RR8qcrhoEGH1xm9i3Vd5qm22d7uuDuN3hsVNCAwpd7w0YBfIaE9NDlFFm7drBq9QlvDRLoA%2FsDGQchz66pML4jYxf4oQTl08hqFdfT0AJuLwpfzxcVDNegt%2F5DJpbpj03thn9uhK7xGB3nrz8gNAqQe%2FI0ffiaLNI%2BXoONjpIraPJR9qLDG4wE%2Bv9GXCULJyDc7MUdHG0JaUfvQgQenkZV%2Bn4viKVhyAfMrso1m7zJYY9y6VuRs3r9BgBcRmFRHr4rjNel7TXbOna3zSZwh5mRQMPrB7rFB4NpG8k0Pcvs9%2F6%2BCr%2BWQGvmm49jW4xBEtBNrFhLl6O0G%2B%2FyoxLoxkrYX6QAlWeDa6%2FnRfKJkvCJM%2FdRqIOQTsP2ydeqkwr%2Bq5xgY6pgEROY64sSUdIuAFBlPb8xZEzwPeii9u4dNsPzTnyGz0p7zmSOO9gmURrTWAboosNhcJ17OvZ5udgCrsfU%2BtD1kjLeHrf32T6C4DtG6uYu6kdn%2FF0tV2C2oyU8Ds6dBe8rE7AeHgnDEfBBnkHGdKa9eawIBRABoONO4Bk6NN6bKDPhq%2F%2FsmIXx%2FH7vAzbvRKXPBrAHTXFBfwvLZVthzleHcoaPUNah1C&X-Amz-Signature=630c0b9f176b4d803b6e3df2ed9d1c6c651ef9b76ebaa3864fb2ca71df7891f2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

