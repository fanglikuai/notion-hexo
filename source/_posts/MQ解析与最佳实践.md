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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663TZXVFUW%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T190037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDSNS9w99tJV6pVZh%2FDEEtSAq30bfPFNqHeN15jG1oS8gIhALAZJ3d8PyP8H%2B1H%2BkIqt%2BNhjk5CFbZHHAdQTCqV%2FWs0KogECJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzLbYBIjPDJuqhWJmkq3AOBYewajdE0yGhChKieUw2oW9biSkkEVrCnj1Tn61AH90uafBTHdGxnY1hSQHmCHxwrmfWnzYUlyJ1oda8bMm7DpeWqidTnLH26%2B3%2BVVyPeApWZ0HDgLzaxRyHdniQ9y2jz8zp%2B5bsAkHcUJHvXF8c%2FwAV09exqBhE2AUJ7K7pzxx9f%2B9nEbcI6bqdK%2BD4RaCAtt6MzWxDOjTkPQFQ5TIgoD11JvakeTwRiaOFKnWW1%2FSyn2otjLfJIGbZXz6%2FHIOXMvFyFto%2BnwwO653BqLQZGs4bWMS2xLwth6tQJ4a4jQRqT4Y9QI%2BR8nljkxei49Gad6%2Bu6YmzUr1Swsa2yqIv3pmDf8yaBpowTfFaPZNtHZeMiRpzM%2BU7k0cwpifuP5LyeTf72cpQPVPm02E%2BABBfB8qMVs8ugQhbE%2BbH2biS9V4dPj2bjp7KqmVXSnSw%2Bj3pUDVu5IV8sz6iIdx7ecZEqXatM2dmdDdu8ncugqtPbEDOzkh9k%2FECo3C%2B3dPoZ39xWxd9AWG7WUyk4TfmS5yy9%2FJzbv07Kwjt1j9SfcLFOIh9cPPdD4Ek3Z58XMezOPhlE97MxIsNUpVEXB1l7E%2FlXJDPefDqaX86rVSACdCRae4I2M%2F%2B22eAs%2FNjmhjCLuK7IBjqkAfWqCZqqDSLYpVCdDkAuvNLQ%2FYjwifrZ4Ojm5kNjYx6LmEbD5pGs%2B4YZnhWaUgAmR99aahZvuhYt6ul5EH4A2vSvkEoVm0Adwa5eZPa4CggR8%2B%2B5GlPpp4kPPLIGpEKkKj27NzQUyayG5V2ey1diIn9sRSOWL0b4IbbVXqBuKuGpjbchoG5uVXANiU1gMDdRd7h47CSZFGJuD0qc7UYQSrS7fXWO&X-Amz-Signature=45f5d3debcb5ffdca15a53df5fd31317176fc83601ac33cc5d1f6ba47719d46d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

