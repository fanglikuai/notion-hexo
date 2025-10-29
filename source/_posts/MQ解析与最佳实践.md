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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663WTXK43Q%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T000044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJGMEQCIHjqFkSOQMzgX%2Fc9qncM8umQ0yeDQHhulgR1pKsqRdM4AiAXV3isHYLD8Fj79ihth6w8asy8eVT5UgcMBH2uyp1idyqIBAjI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2BDrK652v13jYfz4zKtwDK%2FVmB3fTId3iJHSC9gZNoMaf0DZlP5qM2vgB%2FJt0QlT80B9VqpCp3tCIAy%2FFsc0WHNXkLht60WkVcuYITnL4kWarzL5pSU0pvnr8%2B4kqWWprwk17hGz8RME5wIXwVcXZ2h%2FlvNhC%2FmZP0KS1uxYS3Y2JuTCwy6oVTVkQps6BRtkkreKaBopIRgUZT5M1d7TGR5WyQGWRvkb2pe96%2BCdhqAyNdI7oDzRJ1WVLEkaOzqzdbRbs7SokSqQtZ7qa0lV%2Bw9GodHv3dtQPtgh2bcwc7q%2F%2Bf0uqy5VTkfsMP6TC24XI2Gjoij54Hep1HpJHpeQI5O668gtd7UjzE86oGl0rFUxpFr7uomPKl%2Bp%2FGZmHj9D2eOmX%2BF8Yr045UOk4fC4xhUN73LNcbSN4IY0va1WSdGJ0Hj7Jzu1Fax%2F05m7ZaW2FbfW406E1ZBTE%2BE9TQ3xlLBmB8NABRjRq%2FX4edxsL7dL9SfwaiR3TvkK6vBaT1wZL%2FutsCW%2Fmdh6FMgD0zqhnyfMcV4HYxA3i2hH%2FSETvxDiN7pDPIqBQJh3j804HQqza1t9co0WJARGyrt6W4SiYuuidAYcCwXCZ6vTpKOoQ7MpPoCn1tu9oaB%2FDvSWILcKVxKMKnL0nNGmfJwcw0ZOFyAY6pgHxg%2BV8VZ%2BfgWIkcMsbx24Fjx7ztPvjWIDwlL6F8sypKH89jNgpUsOyOWms%2FMwDWe4jZl9yNcBokFXC4YLLGXFPyZZh1S9Hdj%2FfkSy82jZrT8fYfibuQWci1vlP0fOz8maVSk4XN1DK%2F%2FaCsKiWYkTTDssuXl%2FYts7rJwTrmQEL0Ltg%2FSVvU5qY%2FOupbyzGF%2FCoJWCb2bFY08HZZD5ji89JHsWERzvt&X-Amz-Signature=92f24217566e9139114148df57fc31f2cef13779d870148207476b32ba9faf95&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

