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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X263LBGX%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCTd9LhymLrklMJYCUjGk%2Bv5cbFAibKUcpUhlRsmvJZEAIhALciP%2BwjmAYR5slXSlXIpvW8f4ixiPVf0jO%2FnbU2tXS0Kv8DCFQQABoMNjM3NDIzMTgzODA1IgwgOu35BjkEbCydzwoq3APs0hN2aM5hFufJfuRJQNVR2dPG59a0AShTgy2fBhscSSyQFeUu5uwdQ0pGTTWq8%2FtDwSqMoQy6DIBMbsWPqkAXRkcQXfvvSSBVzNL%2BZzikxvTYZ6RG0LdfO25LjyD4BgzIXCtpTeKfOZuFCBHYbnMURnYwkJjkxZ9tbylyDG6Y13dECWbGq%2FpafaS7Xs%2Fgy%2BxssudRbaweEFMC2gWMN7%2Bt4flTcQThWWNQqv%2BYhIwJjVGK6O3fLaTTMKa8UoUbfIDI3d91H2Y%2FdkOoK0MSS%2FDYvzxIZnFOfGiL2%2FHQNLTmzNKgaPNT1bcQQoQ7Fjvnf3VUPW4t3Lm%2FMEygh9AwtN3IPooKtZM%2BOjSD%2Byy%2B4AgmlQYHhyBuZ5C%2FAwpCBa7%2BDmqAjaJX97W%2Bkz8XQj0ZP%2BUeV%2FcVE9dWuvbh%2B8nObhiN7v0nzY84VfCJFrPwS8lu%2BulkuO2rJHzOasX7M4cs1nyqVa9PNNw0UTqWAOqE4i%2Bi%2BNDyRF%2BLvFbIO2z%2BzaG3bNNKHztVmPL%2F0ZxUqpx8Y91RV4A8HozXgGV17MRPtyq%2B2pzTcdKS21WPzpk3gy5YwAGnOTmMboSSf7DuTS3rySSD73n0kQtF7JHv13LqXrhKJtB87DF%2BbjM01EzhHDC%2FoILHBjqkAcO7p76TbXGfn1qWKjlK0k%2BiXLDUwzSQcOtLLytxlvCIjjrW5vOKJCQaBZhErM22ksYY6rp8ppZCu7iy2X5i8IH3FF6Cc3QjKwjKT7letqiljjIgecQzOfRatJq7jvsUBWVnIilyK5jKRd1Tr1b8y26LjwCJkTEL0Z9E2d9JSTEGrHbpluHDYuuZimaNUgx8PJPJGfJZmkPqo9ncFOo%2BgYTKdZ5F&X-Amz-Signature=0e593e0b2786c303dcbc61939dbc3a0c5742c9b1dab67a296ea00134ea87fcfd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

