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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVUGGHHR%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJIMEYCIQDVFfJxfuU5UpD8OuGoDlT7rhlgXRpdY0j%2FAaxkFRq%2B%2FAIhAJtAj3p87NTKXbjA4HV3CAH1g72WVICq00h4TY%2F1AlytKv8DCDUQABoMNjM3NDIzMTgzODA1IgyNWCP4oh%2BrNgozHhcq3APt0I0NePVaJn1Nnij2X%2FttqbhrmQK9ltepd0kPc8u5Gh8LgwAKCPoq31%2BqjVGLgiOasIvCUCeEMcmdQ077P453D2DtczR9S6KjhpnBO0dbJKxKi9XGvyBbIPgGXkmQlLirhvW8di6lCZTDo4QGGrVOxZOAu5YQ0gVRrGG3IrlEEhYvOUY5TZSKbFEhoXGjYHuY7kTM3r%2FquGc0Wh7%2BxPUC3fytCcFjV1%2F0%2BosLBADaTCoiqyXiLjkPin75qGu9HNmlS8TKZvi0dMIP3AGSZBjBWgf93cxyC25lgyaOFWfbupXBDoPW0NF18iP2h8%2FvAUmxLA35%2Bb0LP4r2nq%2F%2BNEZofkSvsvVRd0UY%2FjmoDwCAP9zXu5r8GJIplZ%2F6%2B2mZOYOWyRj5KMjUeeRbZ1CF8pKkGLMy3j6bgrkm5HJVnaRTCvIfT1Bs%2BiMz%2F%2BL6JBhORnyCTTickAifNXSLLrviAfQ4eUMZbh6ScPgjYhMp3t3aJJ871G5Ajae%2FPJnhOSDeDfdmROyaYznYFN1KY5Dbx9NZ0bnHHlQ12oOr9jS5ngjFkbeCBUDBhHiM3Uku%2BVAPH3KHrzWo6dc9NsekLrSpjYUktTpx9W8fE9BVymLXrWbwMdSCACECQ6WiBfFE7jCYw5nIBjqkAQYHweVxycOJ1%2B%2BLLPAkB3AeMMR%2B6jOigy%2BkV9R1Nzu8SpcIWY9r3Xx2CLLrwxqd8PnPI7G0ze%2BVd8rX1cL3s5UZJgcHutzF4YvFBxUIzkeAg%2BxqH8mt0RoWQiYjrOqTCK3Pz94n4HFLig55TwuIGFfOAL2S68%2BGLYklBgtqPDBXbCQGvqj2VjO%2Fr8RukFKteb08yMNOnNcN7e%2BWmKJLlEuips9B&X-Amz-Signature=b77e920b31e51e5b273b968dc6d2c9eb70cb31afde329d702c1e865140348a1d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

