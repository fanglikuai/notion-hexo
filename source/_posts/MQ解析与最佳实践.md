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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QSSYRMB3%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T000046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJIMEYCIQCKoBQ1KPSsj%2BXQEb8VWOGRnnubsfi3Mw7YG7efj7I%2BigIhAKj2vy6nvfM0rminywuXsVEKkn4z26jddMt3ERj9XX95KogECOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwfcmV3PNOOKTqoUp0q3ANRD3wm0bsfnf8yenmNVIQbBM7RlS09kxyLIUvnvYmbhXhAD9NyE46dbn5FlY%2BvlT8Tgs%2Fgcg57Z7FxIBQhdWqB28qQOEZ9nY78DB%2BBgl7x61BidKJx3c6%2Bv5gvD0K9ImxQuh2%2BLJI6qWe1d1QMu2UJXXlswK7ilT%2FGda3w7Uj%2FvA4V5HZZYc94spLS87VZHqvdFNBZxOCRLd0nl07P4sjFMBSltrSPKLiSCYTPA0%2F48%2FuZSdA%2BoxRBCURbb5a7TfC%2FcSlLNMsnC0n0ov%2F3UWvTBYP1zOpualuay8dsQAdOd%2Fv2kTUTVADF%2B4VBFffa59ymRrglwNp9xlwQXW0LhSmr7BYncq%2FdbXsm%2B930U7aKzO2hDBaILN3dRQwaoo0hwf0N9rALCn2mm5EpY2lILE5%2FplTqbsES%2FlTfHegJPa6sARTI25%2F%2BzZQax4TJecc4btqL2MsbG%2BQu5Z9OUQMNN%2Fl0O6HUgxWMEPoRPi5UeQ6GtktZs0VGPTT3Wbavny0A2cRXqaS%2FUFIWobFDDGwmRiz72ZYwkwNre%2B0g2fsVA08n47MvHz7tj4enmyLWGS1Ra5FkHMI3J4BO2NLB5QUjpEm2hG2BGJYcIQgEPLaG0GsZCADbs0I3lBskoUv4tTC%2BkqHHBjqkAQfPIl0vb%2FDpm8CEDJ5%2BVrZv2Ykr0cNjU%2Fy98y8c4Dk%2BvIvZTwknqb6oMKP8KOWwIypYr2xaRBc%2BA2FRtIMMGvh5hwG1JLrOIZHRuCtQzPx%2FByhvTP6ZgNDwxSVtuB2n9yz63xUPz43xzmasZYCrf8PT82ZbWJbhdW89H%2BHH9psgB0uYGJ6crkGWEL6EEeBq9U9%2BWHNdgGK%2Fsqk%2BxRqP3tuNfWgr&X-Amz-Signature=47145a8d59abaee7d4c5cdae8923e461e7598c82554065ab366096e923fb0e95&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

