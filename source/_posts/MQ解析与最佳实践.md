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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QDGBXGG3%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T010042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJGMEQCIAdd94TbXSBiglM6d7C%2FmY9vs6RJTvouwIxhtpHQHL%2BjAiBaSMs8X7S2NHvAo5o%2FQr7fAlmuBsau1xT4HQomIEIIqSqIBAiy%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMCRksZ%2BWvKy5KZoh3KtwDG1xXxxc61GY4uIvsRvcty5mYNy537IaCmmEqZVjJbL4y3ZPuri3DS6w11MTWyp6FTvGLYaEEq1vXaDLU1IlraeypkooccdR1FkE5LYl6gpyU%2BacOwnU%2FuVkB17beQ9IJ0W5WzK0Mcrp7aM%2FIvkwbQ7o8y3TMesqhljrq2OqhCtVTh%2BmK6inXioCGyGAW6BkSzakIXDk7Y%2FOJwaek7HjUzque8l%2FaFUBfYBUGZmZElZ3NsrDIyiGvgk7Fcoc2z%2BfPtHh39pdjYFZ2%2FEw27Sg07iMz3gOy%2FJgH22PvPmW6Vd5rc14mwS3sKNp0JIpTdzNzDMFQvDKiez0NVH3EqRDDgHK4mAYpzYArLEHHEVvvTBNbNnN01DHtqJ3XC2ldO%2Ba9Sd9yF57jRS%2BMOiq2ZzFCBJoiOcC1Vpbbvy6rcxD4J5P3NYlFr7ScWlhMwUtQXv9oNP7SFKdSzx8IPlKNm%2FJycMNjtEquEl4kZgKcWXeHQMO0Grp6zA2hy%2Brfqoyuk9%2FLZh6ntByEiwZvaA2byMVklWs8blnl%2BQkaJHdr%2FdhJVOEcPIdndivk1SytBjKZIHnSzbxyZ8xvITBhwrHP6ie2W2exZ9iEh2yclm3bDD0bBm9FHm6brf5uYnUyIOcwq7%2FLxwY6pgEorSd9UOELMDqm%2FIStUpYOypnsBewHvWiIHyOHirxrUq0vpyL9Nzn9fWXVm4ahTBiu2n7r%2FEuDLKxGw3h0JBYDYKPur9Rx7tE4heLMLYtuT4mj1qrf1gW%2FbeK4lWir1qOmuLM5FK%2Flz%2FivfpTXCSVhNnbPWZpCf92hv%2BHMpvkaXPQZGSz%2Fd%2BKHilUiau%2B%2FZBSgup9tHdwk74%2FnWbYJNez%2FELLwXgA3&X-Amz-Signature=0145804e3b578af9ada6670379f53275f172bce45219752d4e49a5af568c8105&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

