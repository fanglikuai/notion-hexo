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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q3QK5TKV%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T200046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJHMEUCIFuqSsBDCBc9%2FhWpUsaAsTD7XOuhCeu4RfG8WPwPmzGwAiEAgMdgWuGiOJV%2FSaKFwc3MdSpAUO8%2Bw3a7hwZIPaYGpncqiAQI9f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDoYQMWGfWiA7IwWZyrcA0Ni00TGWRwdZwHk5T308SyIjGAbvmjBIYSK9MOrdZFTqnShhURdGpYFZ3HIsAe87U5kc%2BE9JHFF6Ld%2FPwSzToqDeM4h3bf7V26jLDpYpwgPaB2kiuX5%2Bhe2%2BMfpSoYZO0WjqjqQ5uozbmG5OLRn4s8zD%2FeUmxsPhd1mIbcxrJfsgi%2BicFJf6EqAS8YD1mJq28bG9wJkSj%2BdC4njPPwiK869UvqNrWy4l9PflZmMyCwQ%2BnXFZFXq5uOLdSgVvsG8qeZTpUIqQxwDZZtXnNJJDxpO%2BEL2poesaqUgoMRNDIhNptv8yh142vHwSyHQs98sc7XwzsN5P1DH4za1t03QvBSofeVC7AOcy7EzccUV0WVEBVjlIrD282yuV81n%2BAbgK6K2H96lLRtPjL%2ByPz%2BrkrkMkwAwKOuLf5ceGL2Pjynw14hPWZD4KqLUw%2BQINqD%2FScY3tkN28NQusditW6rp%2BMfQ2SB6Q8yefCJSH7GSuwSQiPSE%2BCHrIViHxjVmw9hngPZYrUhqttq4c%2FH0FATW%2F%2F6XLlwj2nf7LAXB5G5D79HcXP9nZsGG3ZRzTOclEZxgfx2YvGuukmaJPq8YunzDoJxGqqb3Yi%2BS9MA0r1NMW6oXMH2aXsf6XF1DtkTaMLWc2scGOqUB4585T4lvUwqqqHThQ6S1MxyWyagbRZ940jU9q6TWePswPZURJD6IgL5nDEmkBgcKmgTwcw2KA5PLbTfPtLPUyA3Q5TWHJMlXoJx%2F%2BSulPXirEsBodK%2BMzf1%2Bl%2Bg9I1eoN0SE48UVez%2FQgb%2ByDYYKw2Jlks103QfxVY0jU8fjpHD10ilUedW2M7VBfF4S3EFqz8KrVhfUJGYw9xOHV4kF9Adz7muG&X-Amz-Signature=113b4970915d1e56d6376a321165a2d4893bb000981f808a85926116a370b291&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

