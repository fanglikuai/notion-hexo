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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665LJLY6HC%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T150040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEcaCXVzLXdlc3QtMiJHMEUCIQCnaC7M%2FzVMVZJCIt0ID78HStx0kDZYb4HhSLlcoxtaVAIgeSm5ooSegQ5isRhAC8w3kRc2spjLgmpK9o06gJrQy4wqiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIaJygwyLxEfXhrP%2BircA7LyYrn86FyM%2FbT5l1Sn%2Fbu5YVX%2BVEGvhioixYQbJd%2BMNSg7T1p7nWA0NjN09N8%2FyKgUySbfk14qvGKWON8tu2s%2FWYWXCx%2B0ZQi7GMJGH8mbNK%2BdPZcbW3sU4DooHbXjRvUmyt7Xgvvmi60LNgZumHSU2zwxst9DFSdnqXXcUgjEjfH0ubQyLL3AMrxormSKf3Ae3L0J5HyT6yVV4TT%2FtOkq%2F5OFCusUVs%2FcyrfdYrDsgGxTr6Of6HZUK5yRIgQMrKoaCBVbIv7lR%2BR1mcAnHhfA9KU98CXJRTmvRV1d8OFRh6JrJaA5ar06VrJzN0bGJR2J6u%2FPG5qeSAsB8TyL%2BEf5PXaC5RBALiIyOxEvLHj%2F%2Fsprf4N6vJIpukUOIwQiMWxidT%2BLnJM%2F3O78Mol5ZdAN0jrJYo2VBg0%2FwLQjk2XNAkANPEggdKNjmbRcpY4qDywgiiaXjUPpqt45G0%2FLA%2BkyoFul3C1PJi3vMubCXzVHofHiwiVfsCPhYyk9MOuxIO36K9l%2FXpGU39wpaffWSbyKJxjSXoEgWysNNxu%2BA5GtsMRoK0HFscty%2BTghjxaG03g%2FugiTmZKVupAcUXTpfZVNF6yZcM3oykyt9qMzWvhyfv94HZ3N9rSjxoMNMIOS2ccGOqUBwEqws0MKhLFk2HxhREDqb1wsVAMQg24mDznJ8OWvIegYVI%2BtCwKw4O7wZuwKrsKoDebqfVov7xuU7j%2FgCL63q7r7VbofslVPPu6pbbqi7t%2FaRqhHSnDfnQ22xisWWHo7m%2Bv0S13%2BEqLP6uveqAbzJ0D9iL5xFPxClivbYbK48e32FSFADt2izY6dXXKpY%2B8CQtZ0iMa7XGw7h9u%2BlZ%2BSA3B%2ForUb&X-Amz-Signature=eec0a50c0d88a62217d3bfae837073124671b5e4f33b4a9c4b75143c149f1daf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

