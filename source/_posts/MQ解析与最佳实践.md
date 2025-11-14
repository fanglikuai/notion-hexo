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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666PAFPKIZ%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T150048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFXh55A4nlPYVtmZW14iqNcIqE10Vcoqiox%2F0Vy6HanaAiEA7zxFGd6FOK2Q6bTsnq%2FpWE3lBs0vN04wO0C0C9pM7eYq%2FwMIZxAAGgw2Mzc0MjMxODM4MDUiDMrE5NvOUKYgWuSDoSrcAx%2BfuNTk13buDrCbgiQC8jTiqSv1LK6DowCDivSqbPXqQyYoP8LV2QB5rclOPU2581qqkWi4b%2BNTcU8qpD%2BFHLdJR8UOCzW8bVzoQ0ag1RfwrEi7ol%2F1c0EhgsMWWLUndiUbOthbDwlVd5DUGL1GLs%2B9lP7Y0YhDpEakam1oq%2FdxsJRaMYLqQppXVXdqO6ibNQE23OvAQOlUCtlUeVFHfl6dhq4Vj1VDLX57%2BAZPJGzjUYkD3w6SJqFYWWti5W1ngzdf2Lc0qnOETFUtkbaOLzS0HIWBtRQwNF8r0DY4O1QccGTsksv8BF%2B5urpniDLxAxHnuelMyRNJ37qorsnhSjwwIANpZxfkO6U9MZOHF%2FEdGkaFr9YC0axDOfmHEgyuG3rDjAF2RHOtS5Vz%2FdxMSnpilE9HFfRcczn75ePIJETj3USI4MtKXVCEb%2FkHkP1HHcVDdSMB3l8ebiYvwXlSEhJXwUYY8lxBMO3jUNWCiXNf%2BLswTp2inAQapr09zyeTsYkKcNBqHy1KMz6l%2BMwnHSvqylJffQjV9LSkWZmUsGwY3eNkO6kWb4TQjt2pDlNz4cAIUS7lUKLOhNxyBU1ifwywU5cZJNPmr404bWZ%2FpahMp1AeetnUTm5MP5HLML7d3MgGOqUBsCvBmMogOCYinJWApmwsnZRFxBFEY%2Bes793CkmmAryXXE6eQ%2FOyeKpOoA9QcCycYd9AgXa%2FvOpQnN4LUlS1Adjmvc5KxNVN0ZRwXlHqtI4QQCXpUD1YhZWa1Xw4StRUVGlXq32BURKMwFaad3rvlEiK1ySHc1kyvddUyTZy8aRdPj65hOz610eeM7thrX8Du7%2BNUSGvcXNSADdiktYbUi5ZWyouU&X-Amz-Signature=8c19e4b5ad789b3231e532dabf7b5778e781879343ae6442c3615b0a17a675d0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

