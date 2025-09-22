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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XNVJCRH5%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T010102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH2HcaweTAijwL1ShJzw6DRM6Mm0iXYPK3LfQTi9uKVIAiA02yZsQHcHJxzUtZ3UfQOOatduHlT9kQMVx0yhGvf3Oyr%2FAwgiEAAaDDYzNzQyMzE4MzgwNSIM%2BtGsvurEC4x4RfMFKtwDhOOB1sQorNIMdchsFxojYqWPzi%2Bfq0QTuLRK8Ml8FplQmAOZv3iRp1OrOAKBIgvTQOdDq6hF2jxG%2Bgr9t1rqoWakGgwznh%2F6ycMEQovfrv21%2FiLhq2uw53isjoAjCXyfikhbIMMemlw1cG1U0zTLheKBnaTIGFPqt00fv5GFyKs4w1f6Tp1yQyP%2F4X7GVKxCW%2FNpLZHLRspPceIUp7tgpPcDSbDJhR7rQe9UC7B1J%2FqGYZVVo4TP%2BHb0wYNu8efWTA%2BF8J9BkSDNCkhSeljsIWH8H5UUVdmXxAuzITocRrTeMu43ZlEWK5JYXyeQNVuBbwKr4u39KjGPyI5jkfjHDzTmg5BgxViExsTW2n6tllmL1Wzy7d33aK%2FLkoKFsb0xJQoc7ADkZprGpq97dinm%2ByqBl%2FjaGKbxPYe7NIbUpfEirakCR%2BbR9yiaks1h%2FZME%2BHEdPrx5lE%2FGg%2Fs7%2BTSWsY%2BkOE7i2cin9cLGV%2BML3kIk6i3G6UcnSBZi0K%2FCEroJPQU%2Fe10xhaQCyKeYaX7t53%2BlEquxZefBQNu0Vqa3NQ9gFj8BLVnk7nOd4fj1etRQ9e%2FQpa9vPdYwvSDaMz7ftrYj8o%2BRJ9Vil0IuENLLCIh24wbxH2wGS3OPivEwn6%2FCxgY6pgFER39B2dHiT%2FArKxc4%2BNILMHGU8DYHUO3vT5ypO18A%2BeAf11NRouc6WeF%2Bjf8eEvu5aXSfIS1YXN%2BQ4Ym5rKZHVSp6KqS%2FtvvM3%2FRSzyfGoL9lKxgk5Gi%2BS6SqiPXJZ9rO3R%2FcFM%2BBE0SwUIw70oaxi38CSGQ%2BbegjudvD64LwimJtQoV%2FbOEA2BDjaT9jQJowFUCbj36SOT3Ad4GTcw2JjTnqr3yQ&X-Amz-Signature=0a623f4701489a0432cb4e89ce4d766032f674c2b35be4b6aeaccceffbf9954c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

