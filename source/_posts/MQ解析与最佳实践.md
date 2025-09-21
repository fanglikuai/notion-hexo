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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R3FMGM7P%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T030039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD73ZI3J1Y2%2FknmsYKNexjSgUOpX%2BprI8uH2Kg%2FvzPgkgIgdf3ykqU8B1BnufPooT6JIDJ%2BC4MRqPwbisM8d0mfVKsqiAQI%2B%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFEvwY3a2%2B4S0YYWFSrcA3wTqsDcednZoesHeqr%2FZcUw0jJ%2Fxz7%2FHgaikkbIsNta98k1TzyxVWntoby90webOObTJ3fmT2YyxywKsP4CA2%2Bsd40hWHFvWnF%2FZskHohaXOL3zzNMqvHqJk8zURYp4FTU%2FHh9XAAzdjmlcj7cZp0jBdWPZO%2FHYWpyVxRWA8LVowkqrHq56klLoVAIye0VICN23%2FZuFFmJpONs7LkVT6rdLjDkkwgiHGUJSlQ2awX1I48RTapXzHW5dT23W%2BLo2uvBmD5F6S7rC4YnpWA6Z59mv5YoUSXG8igsYoFFScobrd8n6FNzpAOcQby4G0xFb%2F6qurcj3g4a7iXOuhoIyFgSkwLXIQ7VDbh0tMJcGspTHRviHdL5rBNjXlDqjpcT%2B4GGY5iBZqCzdGN5tMH8xfwdTZlPss6EzG6CwKgyAsn0L%2BE9%2Bc8IZw0YzrqBAdfhbwe8rKKyMybIoaWG8hWDVXgngXRs1rW%2FgTFDhlD%2BFW1%2BOh5ggEj%2Fm0ihSVVYi8nCvcN2zRN7I4FiJzDiCtw8UH734LDL0Je8DO4mhvTH2ykCc%2B1JZfp35gqzlxGgn8fXKHNtyXFTgQjNtswsWHUGbKrOxFeLQUDNDsRwsD0X4aV9Ln%2Bzz1AoN7AO57W8qMPK8vcYGOqUBDT%2Bbwfy5m977AViP9DFJ3APCR8tMHmXn%2BQQIxPdYf5q24W9hko44uob0FYXrlWcFtYuJJx%2B7hOgCjIrn0cYU6RXD%2F8oWV42mjCEo1VEaegxiNdiNuIYtOXPdodX9BUVeSt33Y6gV3kOa1lByPJh7jf8M0oODnjcQGX%2B1S0ClzT%2BHlpRfi34p9NlXET0SMd1Z8xioSOd8jMcxtTHW%2FBkGv%2FBtkhsR&X-Amz-Signature=93b2e17b3f731949611dd8c36343f37bb74e6bc726d698fd6fc428d9fd6a7fd0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

