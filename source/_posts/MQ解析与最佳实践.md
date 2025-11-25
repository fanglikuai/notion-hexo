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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663GI5XFLA%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T010049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAqe7Blqkf%2FQFFUqL5p0J7MQNFk3am2P34Fa8lxHJPJNAiBCV1SsRIX%2F2zHnRtc%2BrfSWzaKLMWBT7NCTDNjy0WAwqCr%2FAwhhEAAaDDYzNzQyMzE4MzgwNSIMFdHZR7p065iy%2BleeKtwDQFp8kLWSZW8IdYsxxTzR6zKpgNfDOngqea%2Fwt7bH9zAwl3Q64Z67TKLI8TL%2BR859D6v9jZmRK1qdKb9mAj1JS0YBi455%2FinfksXlQ0yJVABJ%2BBwY4%2FJTfLNhifEQuyZaBxpYe9bXX9Uk4pNiI2qSTAhL%2BSXRYT3HplPlx7gYlD8NOHM3tNXmioEfU2OeK3G87nkRqFhxPOVKZCc%2BrX8SqrFjKPFEtKhMzVq7W0UNv8tf4fGmHk0xeX4UU2W7n9kPkcx4il9bCaD1MHzxRYp0V2KkpaPJ%2B%2FYTrwv1xu%2BpM59TkMWjvlkDbhUZPO3fvlqJsWQ7qO%2FzOMmKOsYDUiR%2Fbi78Kr3n8UbeYLWn8iluRB5NzvmKfUIzuHAhVev4s30PpOUL%2Fi6nL4RlzjfmGgYxI6y96uDsYwlGRfo3QG4EIXOJWUMSMnYmCfjYYOWpRsnR0dgV6CWeMbi7m37bx8wxOdW9dCW%2F6aZzjsNrfvU%2BhWJnb41Av6W6tNlPUBM%2FbWdQb347AfQ%2Fz2i6%2BB8kOeiW9atMGoMxcBhXuROjfhi5hOP7c5x4lJ%2FiqylwcG4XsfZOHcmHY%2F%2FGYnKtxurYdGBdu2uSwS4QxvdOLT6UQzV5KzFcfeEOTO0hRmHvzR0whteTyQY6pgE9mEFC3N5xAhaa0P0dpLLXJ76%2Bf1DginoOfTQS29kDAHHw45QaRFFfHF61shWwyKhFkYI9tYW2As67Dbto9NR5iIyab9wQL81TnjG9QM9q4DILyEizuHNTaRIw%2BZMfI6JQBRTpMtWxz1gGqYFULnR%2BSClVyb%2B9B0lLNywFkkqhwS5qWv7PadmzhqMpyIDoEITScVrqR%2FpXNaIMMiHZRK8kSLXI2xh6&X-Amz-Signature=ce68c020554e6b139993023f74c4f18f63c31660e8a3399f4c04361a1fa48633&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

