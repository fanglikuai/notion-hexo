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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RHLNGGDP%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T060135Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCLD5VODVh2TWu1hCW4NYUSoft6SGJ4P9qXf2fTLU75ogIgNyLeYTYS318zEuARaydqJPbWGjrv4XNjYwcNqjNvdCIqiAQIvv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCwiwGsOQgakXBCJFircAzhodBJsK82E6rISN2yZPcENJHaDfjxZ1e6URsKGx%2BHaGmIAdhTet0CuQznJO3kzXS7WmMBhl6UgORa91JL1R6pTEekPktz%2B47%2Fh6hQ7gpkpioEb9Zm%2BBt1hgaORWN9%2Bq7S4QNJBGSlGB%2FnzGVzWXZOl7dZjvbki0nhafRcd7WCH%2FbJfJKQ5i1BCjqMAMFNyoRzBde8PlRtvn4k7moKUbIcH7I8lV6H7PY2SMqz6gC4HfUt4am%2FbBcmfd1yUk2QTAdBnkZYIEJjk2b5gSWV58CWS5Di5j49TIxTALvdB0VWvlrXPJz14h9BZnqJV8Eo4cT9za2cxjWnuwVIr0wAd%2FEcoqgTeAaZHjkAJqkQU8zik2WRXxQxheCGil8Cbmn01WLyqVDUebtAuBv%2BCxo8gxR%2B7ozePRDuydYVrvJKiunH6B1ebmdU0Kqo3JqZ35nUi246zZdHtYI%2F6lmNDC2fG2QrwHUU%2Bv8HPF3XDXo6ksVvlLtkFK9FlGnlVmn1F9cA9DThBaoggQdeGid4tA1KjEtbRICbdE4pmj2fKS1uN3I%2FYU7RnsHSHlUBx8i%2Ff0AGNz914cp06OzXAe3qLJ61mfrtI%2BaHXHl4Wjcrdd0PdZPpoW7Ex6B%2FyrfLZ%2BNG2MIL878gGOqUBLudDmQNhUhZIENVkpasY0w4wLXK0BhYDYl%2Bj6PEd45qhtJyoLMIGfZMehETBCzc%2FDWCM%2FxSo2LTH1se8tXjtGISJ1%2BZd8tolcB80iz5hioPMVKQZP3ncBUFOJKpdLy5hE9RmtjF%2BYKGLT%2BKzEk%2Fu0IMHZ%2FDDHYYFmeroItU29WpbRXJJ1KDhxZpY9B%2BJoPmitBb2iP3osYNjgr%2FU33tTmmuOCP0A&X-Amz-Signature=96bbc037b8e00c2a8f6e8c21a819f1fda1d33c24d387e579d0f2216597328b48&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

