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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663TCS5OYZ%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T130053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJIMEYCIQD6SPWqaXVSPWoXirfRKUmqlw65nk1hYiaT3lN8QCfR5AIhAJtVlZSreHvT0JV4Urx3XnTQBLZOZdk0fNU%2BMTzjPSuFKv8DCC0QABoMNjM3NDIzMTgzODA1IgyhaLcAZf0HbPWatHgq3AN%2BkwJ1QDo5%2BQOAepk2k7cksBuloCKVBqOoALPez0TOobW9g4hm%2FRoymaUcAybxRleoBqbAcKsuhWFFmfgu4kdCw5Nk7OLTBcb%2BQnfdFsTITD%2BfywzjttbGYUwkpKb6Vts7EWE4sXJztITbVgWEbCQoCs4h2V%2BEvNfv0Mpm2hzpWMzJfBIix%2F0DekayjQ4DwoHQYBctSZdNfxUj8pLc0lmLKNRk4kgWa%2Br%2BaqJa7x9tGnMOpNPx8qntEoESlz%2BnyADs392XT579vJZmQxow%2Fo4dCmgTmqWlDRfXyLRrgqM7T2HG91o4yQbO2MYMw9zGpiBomHKXedygRXWLLf7OCMDVq81ffqAX8ORIXwlLxw%2BchJN40%2B0WX9rLzPZOQF1HvM%2Fspgx8WadLzZGyIo3A9WCgNEAGtFNsHJYAd6tLDpZ9TO5hVGhJzEW7VCWB1o5hQdv%2FzLdPdWsCG4M3tYMfY8E%2B20cp1E4x0l%2F4M5NWhoXUnLosY4cYzo7B53E%2Bv9p8SMQOIDryF7gWJrmZ9O0ybVnrvAgMT0riyLdgZM2x6pzuysAQoDMpNdgttdojq5VpF0I4Ft3YnSq2%2FPVPu2ywT%2BjB464aMjEO1gsaP%2FGK7H%2Be58n5aXcXr%2BLYRrEPUzDv8JfIBjqkAS%2FZ9DGb6u%2FuWp70qhoIfbKMc9GeUd40b21s1J8EJ5Nl0drrYHp1RdCVyLz9KSVkt7zjjdTO2%2FewZnl5htVr2T4d9uePTiK3aY0k5K0DQIV1pHTZhoLbUq0i0Az6yhw%2FucSOrMEJB2Ogsn8NmZS2ar%2Bejsr7jH2JkL7s8aCa1rMi%2FS5BEtjy87u0tmYZfBujM%2B5HKVuSbCoO3NRNzJIC5SmzhKic&X-Amz-Signature=fb7d8124d02902f0e05824f2f42f2fcfa6a7745523b3ef912db9fc8d4c8b7f13&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

