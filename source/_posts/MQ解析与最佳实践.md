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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466672OJIPD%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T190047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJHMEUCIQC%2FK%2By3%2Bo%2B%2BriMcDR5GKTKidA1AHt7jgccz%2BKOusAmHIQIgaDjt9sWev25KDjuUUzr%2FHc0lNV6TnDe1PWzJATc3aPAq%2FwMIOxAAGgw2Mzc0MjMxODM4MDUiDJvsSMGPvu4GpHsFlircA2BRu8ix%2B78ggbOZhZbVnNGsIrjv2TH%2FFJ%2B7XlkpAV3fJfVNmBizhuWaLok6COPlZxk%2Bi5B0CjMqMVKFc4IK7fzqBghcOI7MKFEj%2Bf9n2HA6H3jPjgpmxa2v3uioMs9Wsfg375fIAC2EwFhabjP9BZsjsitXu%2FuR%2BuCOkQzE4YqU6UzbGi5x2dTSnoBKZImhYm%2FhhYNUn%2BysTRlpPC92o8zkqtCBuK5%2F%2BF7BONG4XFFK7GFhArY2%2Fb0lHViMKSFfeUoed4NWEBI2MiBT%2B333jHrB5F8rmY7gbMbca8AUpv7r%2FJJaJZuDDXENbi3myeA8UAB8OO3FZ5KxiakSTJOw%2BGeoRQ93pRKhKXt7h5rJr4Ob4vLHdgZ7KU15d4k7lMNgQDfSD6vGpmT7x%2FL3%2Byh3mevCVjOFjGpN2y%2F2RmYxDPTeFsj1XSjXzDPDdBbjO8xFmKxkFsJZqge6YAc%2FfPZOJC2mP8aY%2FojWO2P4id8Dob18D0p1vLus10EBF2npWbopCFI%2BD7nIxef3VXnq1iYvSnqIQp5P%2BPKxuh%2Fzn1XcOR4O6LZ%2FpSdMQ1rlGikynDEYkz3IgM2b7bHhDKLCP%2BBRYSx9Ql8Wppb39etFfCNwRFegvUI2AQ4Fe65fvnUaMKub08gGOqUBcJ5XpxDyvSGE%2BgXNNh2U4EJo42WhfEym3OJD20IdxjCLLEOUEvaZnBV2lrdBsBfIDmYyR9y9rNsqKtdA3IySH82ytOfnHWqCus%2Fy%2F708JTrtirIJuGSp1p92VHEP7p1SyrnHIudOFlzsqxo4hihtMywcFLo8%2FB0e80BroAbYAhxhQy2n0hW%2BHFJt72uytheFEInauX1Q5YpbFM%2FxGA4y4qu1bGz3&X-Amz-Signature=8517b88b8df64ed0656dccb9ee067ff6cbaca7c3f0f93efb7e3dc9c71fcc32c3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

