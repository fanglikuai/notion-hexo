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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666CUSITYP%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T220045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHPljDJn6I65VXnuYnVLlj9hVCpeJ%2Fn9OLYNHc3%2B5W7QAiEA%2Bs%2FmA5u4cr6VVcHq1eFTohsy0vN7DCfEXNIcsYhQIL4q%2FwMINxAAGgw2Mzc0MjMxODM4MDUiDKa2T1LuHXlMsluNACrcA3lUkstnzE%2Bk62%2FJ0GTAEjhoeH2mHHjoGrHse8xFK%2FOyXKtceG%2FB5yZuZ4eRTa4xoY50E7aDcVmhUsCDivVeDG9EAR6zpgzlXL4aTPgDzNadbWrXiwZeYl%2FA94POvZxUIdQCzX3TX42UD0lh2pmpbFacmo%2FL1TqrRqqsAPvWagmJSltymVAOeaCuz85%2FcDpZ%2By%2BT3YDfpwyYLYkosuRInvr16EjA35T83MKO2VncCn0AqaQsgtL%2FfN0MRiWSx%2F1vTnjJRoYYXArVc0G%2BxoFnGkh1UhzPu%2FUPLgN82WTFQ7tZ0pxXIRecsyBgR9KtklncapZQi1prwPNiRQdiqYXaIi%2Faa0P7jHdQgmjoyxXvQoLQ5lHFX%2BJEftTJCdRX7pfTD4Cze%2F2pBtv033sOCmup3w5nx5rHgkQM2J7wK0dPW%2BL9uY4khZD8u%2FIAqczokkwY%2Fa5gWwIX5R%2BwXSDo3y6ndNPU3UHLG6p0%2BZSznBmlCnqPmF%2BcTA66VNwicsaihmz86LVXf%2BQJdtzD%2Fb24aDM20aCZvPwaHNfpJJZsFK%2Fw%2B%2FDVSbem5AmsnNO%2FRhHy48fUf984Q9Ww13r4AiMzcww2Hda3vvAyJjEUVztMmUs5Odp5nAoG7yY%2B76kVRSorMKL%2BxsYGOqUBnzbBOZeZZeDRBrHKq8U35nD0Nkg4T4ZW5jn6Gvg827zFVLHHgeWXUfVfSxhrUG8hl274KNOvBrJQc%2FMQNweG009E2k5wi7A1dCW5kttc0NOGLjWGxzSPGSQCv33dqeyqOQpJ28oVr8yUpr%2BddVrAcIJ5tr0%2BCekEkztmjXoZe1zh5AQL%2BNwIJxwmQ73GT9vzWOJmAALclBhA3nnbTFbVBBpPQfrS&X-Amz-Signature=d2a5e2def3332409b7dace6c6ea9f6caab7116487d72c366d7ac9da7e514122b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

