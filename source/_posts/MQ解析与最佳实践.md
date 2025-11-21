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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VCRTND6C%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T230041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJGMEQCIFDHCUQPqQOtLdwfIVdxU0kyA1rXS0p9L8EQhX%2FLKcJPAiBWb6taLUVrbdoJQeX2eqKztQ4KITLEy8hgs0GExdcHJir%2FAwgXEAAaDDYzNzQyMzE4MzgwNSIM8s2YuJ517uoaJis7KtwD1eRb0I7irvgORz1e1uXQdNSL4cCEEur1RFh243fHmai081ZOISlekoaIZ9WiKpDsEJ1pWZNt73g%2By7wTowbK1cDabCg4RXUBLHk45wcmSLl3iXxjVZMc9ljBN1fKLRj0H3gw743fA4aZKDUCEt5p7f8KoCGKsdYkm4hehm0rwQSr4C3dRiaeZ9%2BgrXZpG8sSb2DNTq2UZj3so7aZ1jSEBKSp79OfdqhAHjX4%2FWUh9keQwS3BmFu85Vx8hJbvaEXWL5jTbB2yuiJ3eAMRgkqzsfnIhTAVO%2BJOL%2FqL2wHtXtns94PPkMAc1zVmJFgumET2iY55BndsoWvxBsEaIz5JhgTMKNIlimk%2BYky9ZXML88VpV3if4gkARjooN%2FqPQdYQnFBUjMH9YYxWao29KzFCqpnIR8DzMUMzTJxRDUamTPd0FKuUbKtqvnumhRb9T46pk1%2F%2F4wFQHRLZCE4pRIuxpkBhXpwNwgM1PZxNj2fTJVf4Oa27yTGkEEu03ZLCR51fR8pBwas3sdGBPXo56k5xsiJK1mzvLDjo1fEAKei60U5QsRxym8Gz2tTfF77DgFJSbYYmw2LY0BRqsadmA9RYkfFKzI%2BUU8lXnJtyjwHphIuZwEd6kjK6oKX5Fsgw1MuDyQY6pgGcnpCoTWlL30cv2QfOf1VjsmpeJKXLGkhtQRpbztSZDknsd9BheRitqAXX1i7WELu9TI6XTEJGKzBHa5Y4kryd2cZNlaBT8HvHvbHmpMICAwX8AAdy7ocE3GGVrFxqXO6w62ebI2BB9EvsPM%2FKh6Gm%2BWu%2BVz1zcpjtt46ZCloQUDrpmZSbo8%2ByP3N6x19UnovHGy3cGaz2iRgxkNeAUYk9cxEpp30e&X-Amz-Signature=934b9cd5335e7310c770f18e00cb52c7e60e29ec5f2534baab4531ccfeb9672a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

