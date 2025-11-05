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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663VOLOQYX%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T050047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCwA9D2xQn4Bbktly1eUcQqgnGwvW%2BBnqu2VWzfHvhARgIhAJd8WQ1Oon%2F4uA7tAXRRspw1kDeu41CeLnWaafPJf5cCKogECIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxBfxRxV6BdLFjJjuoq3APG9YLhirfWbcWlYoALI%2FjhCOz%2FdYOe2feHS4Pn2v91B9EDNstkeUgE0JpC99sgWWz6CsplXsvBH%2FC7zEew%2FFNAtu2OGICb4l%2F5HgUhC%2FgcjnujAuXmzifbW3QBdHGl0XvHb5C%2Fk%2B40GYa1bhyq2NA60voK92UIHXsIrHWHW3DtktMRZJI9mKxjrrYEDV%2F8PXkemC503qtU%2B9jecPPgD9vC%2B3qru3vdGVYmSuIOvR4VxCpX%2F3JGstBQyqKBrTI8Qc0mJijJ6PceglnLmi6tTwBgM0wpwTslKQA6uSo8Cp2oH%2FsLsSWmbqNu4%2Bar0TBmm3RsAZaXaVFee9lUQhtpGLbt0U0LvKoeLNo0qtGm4M6ApjYsmadzwPvVpGbv%2FIDw%2B3jPF5dCOGJWR8eeozRTVtF73RrnUdYcmoCA%2FqwkyA4BJ7ToPQKdUt7rmEksxA%2Fox8u2UPP4nje7zU408EzQa4CAHiDwGeitqTE1fJfio14zwk%2B6UQa3%2FCDMbgBhsTfY18oY5yBgiY3PMmiL4QFswnhPZZ7Dl9k%2FKmfF6%2BF1zLKMUf8Lt1rASUYNUoJur61FpEFM44swBVlV%2Bxs8Cz6Ilzw1YT7hq6ABE%2B4wKK9MeMvR8uETc%2FAj4iVhRCCCMDDSoKvIBjqkAQeHOKDsN09alF%2FHAFuZYLN8uWRNHzFxm0nqmPKH47ePhJZVBn37nsjzev1C8ypYiEgaRGvzw3V55RoztR%2FZ0G%2B8hw8bJe8MZwXu%2F%2FwZqBL%2FP06C2%2BFJ6gf3W%2BOCQtEdIVQvf1LvI%2FAVMXQ9urLk94mL6ZhX0wcYCGTaG0vp57tNln1Lh1e%2Fb4VQ%2B%2FaIrHIa5UuPufOltKwWkviasdLGov0aU4bV&X-Amz-Signature=dcb691f005782798841c345a5938bacab0de2ce64fec8d9f8c7d0329b5680837&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

