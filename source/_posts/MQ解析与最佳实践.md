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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667RPJXX3R%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T160046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAmHF4JyVoRle4jvqCEVsKdlbYxGAvjrOJ3E6fkZW9iTAiAYVX9f5G3W6f%2FnzLjlUU9qBo4L65yzLrGOn24yKdOm2iqIBAjF%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FVzJbdHfUAHJE8WIKtwDOuiJd8XleoMEJU0ElIn%2FYtF4vgrA1RMwBD7OouGGcaKnshngOOXptMsKAOLW6FJmo8yUV0I7cSqCOF5ExGpD7HCDRUtyfVqseyrdqAFKKUP3hUi%2BVLFHZmpSG6JmPmYNOOfYAZQ33nKfEiuIN6Fdehx9JhzosdNJSaWAbDzY1MxCN5G8cx9VwKToEQ2wfp0JsmlIH0c5Zp8sCPBb8nqFh4S7N97Q3rXDEoATIbK9W9pBdNINq%2Fw3SjuAjzaIYT374zraP0vbZLo1An2EJHbyplP3smC3gL2JynYYE6vd5ISrfYrC0ImULMtKAEuLJquCaWCh3rCfEvF%2BfOd0PH0C4twPXO9a%2BQK8bnVPYPs7UjyHiDsNDbiy%2Bme1mAzeZME8dLWWG949gt1cIShQ3WkqbFlcv7xio1LP%2Bo%2BW%2Bsy7gbPSo4hfs96bGkY0pP6lDq6mcFkLHX3EdyJNlJFUibE6ZaZ6pamkhiwWEn0PiCCEtfeHV5nB5pXlOYgeQBGiBch8jXx6K8TL9b4sh%2BVtQSSyVnUxQfYluhmVW1%2FjjoTxc7FeFjT0aMxPf9YFuOdqh%2BfKpd9BZPmGcsSISUf7qrODE4kYFQfQc6TJE%2F7WVWgswASI%2FvC6%2Fomd9MrsBa0w6MTxyAY6pgHDYy7cHLbFRb2DB%2FreyLrDiCFi%2BLu36tZvhNFC1jS%2BLkIWl4qKlY5ZyYuOlQquNbqX5U4S0HcyoWpYoUZNfbUAiPO3o0e%2FIUa4mvFzUxqnqX1AIWIkEBi1vYxyMCE9IsGYIAGdvippUZc2NoofpbpnVNjKUbxNGNu1R33d5tYX5jof8tdxOMB78C%2FJ8Bpt%2FdZYhaP%2F6cKB0nawxXKcnEfSP%2B3s5n3B&X-Amz-Signature=0c825c9ca0c492aa4c670c9c76cdd922c1f77c150e6cec60f684c256412aae7b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

