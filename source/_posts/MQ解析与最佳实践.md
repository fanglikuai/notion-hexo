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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V2HUIN2M%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T050040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvZGmo4Bv5M1JqIcl0qmFkv1dfRH4NzaVRxkQyCWgDJAIgJuEdMuWc4iRx0PYVS22CP8Oh%2BtcIvVOtfHVn4sID66gq%2FwMIVRAAGgw2Mzc0MjMxODM4MDUiDBnaLklzKPnEMs4kkSrcA5%2FcIEi9ONE%2FAEvqYAdg%2BUH9oF05g4lEVM7Mz%2Brj1f8A4VErsjHwm8ZcSCdr11GU1cTbAWxhcgzBD6M2Bi0vauU2AWB2R7HhnAuikEEJ6vNr2U9kTZeI6XUCuKcfMHsbn7IFCnW%2BbIEr%2FkJax2JgVarLS0SG0Gl6JziyS8SQsrSBBSZo%2B1ZnSn2hYAPawXTgW0TADnRSV07liwYHlsmpMfemr%2BWoxhbhP67uVjcYbQkVPIPjfyfSZg%2FOevp0xNM3yxb%2FWc1p4i4V9%2F2oz25TfGy2VK8%2Br8DHzGlh0ZjTysa2xgvSEL%2FR42lM7WGhNusnrhiCc2puF9HnV9wBxvN4RGYIyAzs%2F2e912V2WVTXSGgj2Sov%2Byi68117Lm6IpM9TJdfkRl8Ka4uqMO%2Fcx1pTFQkcA%2BC4jqc4jf1HW3N9yt1BRXOt15OiVYV8Fb%2F0M7S7CJStnzrtb3G7PR3yBnIfKSoAcDozJ6uSuyN6kT8EMWU0bp0t1Cgl6WhVaL%2BPJLFFUY45zj9clo0M5Se3y2eG8xhkyOPMrhrWP4qU6wAXATTfeP%2BxmZL344fIs5P0nWYGgdJtnMO7kA%2F%2Fs7JBy1fkl5D7JXx7g9Eeg9W08Uyf5boduzdLKYoSqLWB4I6sMI%2Bct8cGOqUB3WUgSN5pp9RiTfXH5yMbhlN%2BiZM3buvbq%2FJ%2B9YEmU8cMnnsgbMSiTdX6PPnY%2FZIV9hQXEDXEvkwpdDJr%2BIGAhokaMCjdNRo1oHQh2QseqyUR2nyIb%2FHXw53XVK9%2Br3pze%2BacBCvq7c6xZvrXmu%2Bq9MjzdffdW9A%2FuI2vYEeiq4k4rxe8slLaHsMCtCNZK42Qq85F07A%2BZJWBn62%2B2V%2FPZqsCfPXo&X-Amz-Signature=297e4f96a9826a05e263ed81b390ab122d06d8502fdc81cc4f84a4a4321d3d8e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

