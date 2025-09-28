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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YPMNT4CG%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T180046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJHMEUCIQDTv3GEQ3VUPwE0S7eKQKqKJJpUbJvqox9nTd1McU7bAgIgWTT0yPqprtHRSN4xX25do2V6s5lwE%2BJog55hgznXM4sqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNK%2BD%2BjFLtDsmn%2F8PircA3%2B0ObGOcma8IM3r3F5XreBC4Bi65QOuRyghgvPmmKivHbt9qh8XGIXZ9p%2BZzfME62rjpvlKYdZw9CO7mEDlszkVXJeRBtSpMc6h4aVavhKGoGIt%2B0KAIGZzk3YERe6l2c1dtmmhO5aLFWapjbBY0v%2BygrWiJmuvH2yFEFpMVs3YDNjWgg0iWvkLYC%2F5jJy26TtALj%2FF84beTt4VZ%2FkI8YRdr82ScOYMJeQvA4j1Qlbplu9mHduFfiHXF%2B08s3Thtq6EysV%2Fo993OleXrf4%2B%2B6Ui3Y0ZcAmabNh5Pbd6Cx46rZ40m7mRlDcA0tUCcpTi6HDIl4At%2B2wFKSXsWN5sXYeaT1Ih9orzmxdCoNg49tuimfWLhQ9pg2SK72xkzRTjYa0cm2pJpPRAu%2BUfj9rrB7ksWgFjpEyWs73Myyh5aaebLVPjYBb3or1OQYURuvguKeCocLINc6o59L7V1bg8f4MLQoiUYYOzDTfnQvXv%2FSo%2BSsrEKuROx3%2BzJvC%2BPjT5pujXcr135gNulBO6yHdDPf8zZ1X0106jZO%2FpIYnxW%2FA1mlP9y76gvqu9W9VMupaezQqVIjtXYFQvihvMGeA2eyNQImFTz9Lkxow4y2s7BTEj8Yvf2auTgm2nkiAIMNDX5cYGOqUB9UHUUUTJx0HWJ5t8%2Bv0tJ%2FlWeViz6Nnlg%2BfIoHOe0hkzLI6RZQh4OkUPVn97PcgOYHKYNUVWKm%2FUQnNbx5ov5JW4mJcya5JwcTvcbJh1tjJ59RI%2BWISJ0qTNQcG2zX0CrTGB%2FaFg31Yc30X04P4ZR0D2V%2BjxhzU1z%2Fq55W4t6UKqMZR%2BC9DSPDek0ZIrNLnr0N8cx13AkWg2ir8q%2FvNJ3JO9fgkC&X-Amz-Signature=ea57b0a4f119c54be8b9a918b10586672cc7575ddc9ba4a2dff9e4562eeea4c7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

