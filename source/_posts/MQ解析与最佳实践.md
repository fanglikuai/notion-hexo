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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663QOPTCRL%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T180114Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCxDspVErKYxQXzt8cEw6AQc%2BkUbWjY2XDWlav2%2BO7n%2BgIhAN%2FxklsPet5nyyLYlFziNJys6XDkuQXble4rT0XGnF3TKv8DCHsQABoMNjM3NDIzMTgzODA1IgzJVBGL19cp0Q3hu58q3AOo8UvZCPZFniXwMTHzU7Vio3pUHFQgzmG%2BLIG1FhQq2XWxj6jhQGawSksVw07feS%2FiQYnlkpaTctAPejj6zB%2FKDsdbQoTSdGHLcFpgVdFOgaZPZ5xlrMD5551oiL7yNPvSfdOYAjG%2BHF2AHJAZSGW8B5x9vG2Z6IN%2BNZNtqMCL9f8Y3vDDVr0G6RMttZEz%2FfN0dMS%2BxblTBHPf%2B0HtOmciMlztkxObmtgq2y%2BDI9Ky4y%2FdUcwS83U5SrSlI5WjkcUQ3SS2Roj72DHOurVb%2BjGBw1YgpRDNFCUnqFSvfx75ibN98yZaO7Hn15VWSgDRaZTrQhwg8zyDvm4dO%2BrRS0hUM7MI%2BN7N6GKo%2Fv%2Bajdz2mU6G5YDRAY64EzSuZyAYg2mDqhJK41WdTBzOtYhx03ZoYkkSk50plAun6HeyhC6W2EUAgkllDlDR4twjFguD2w%2FbSIj6Wn2MbMIjKgh7FTY2f%2FFGsPM0aFQKmXV0yyZDj3tNMs0RQ9%2FFkWeR%2Fhpf9Yfl3COxYtPkAzN%2FPuNEMkMBhj7wiq2oElHc3KMMlSzRSyjB7hni%2FM1b%2F2qXqCE55%2B8TZh3dDBPz%2BKjmQ9TOuq%2BRXfzxZ%2B7A8wYJug4Fn1kNuRl61ixYh1FTZJoFtzDi99XGBjqkAdDvZ2PPUi0E8IYpKyIt%2F8Eo1U9T4xx4lX8sNkmJOai4chgGqOn5rZthT2j4rr3g6kQDz5%2BSXjWJloSPPJ3yxNRAkLmbgVxsOssVXy767XVNE6rGu4kCAVlnKPr9NjEPxUVUwxMgGHoyEG4EYSwGB2G%2FqemKXdUun4g%2BIdQZzmxbSfySGFBgpxttXhLT7VN2IWE9FDvpz1teTnFePlq71t%2BKi%2Fub&X-Amz-Signature=355d8b7646f615a3948f5c7d69893059ef0d3597346da63566043e613c39a2f2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

