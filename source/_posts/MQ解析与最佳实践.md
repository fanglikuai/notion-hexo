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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YBQRPIEE%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T000051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJIMEYCIQDLzo1jqRfP1uBl6nodE%2B%2F4GqoHqkcCxh47764Rtl8VIQIhANV0SsFyovG4%2FDNQ9cfT4poVnJk3jt7chwehaQKpj%2BJ2KogECMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyIPWS%2BeKOgQpXoZ7Yq3APTCp0JxAmVF2%2B5kWr8NIGCMXBMORWwxFcrqLt5GbNWlr%2B2%2Fys2hGr0rx0ZVcBAWzcpLkU2yPgdVqJ237FE59kVLwp4RD1mK8iGUQHaUfGLffuh22yefZX1EAk%2BP97KjYXGDP8i99Cfp3l8oM8TNNLWTJF6PRVO6OnlM1AmDmqzkwQBoHbSz2UriZ%2BYI2fhkz%2FLnIjgoEwfDYIghXVPIbHshVCprkL1yABZz6IVU4pEVM7IZNvDr3soiiL%2FDeNttL%2B9o6Tj1aaTvYpyRg0L2DzzNrQegctQnJqcu9nDWzdiw7KGXHJeYlETOQlx3jQcVUPz%2FDuKuAkvEw9Qz%2BCkIGm8VtItmI4XWdcpL211dRKJR7R%2B4u9QWldXGZ9DG5dbIoiV4T5zVaQ5ED%2FcaH%2BjIKTF8rM6rsQLsSDRmIHGUx85p26AmRfoc31Dx2mifnuKJatrLVCTdJnldB1Lq87e9%2BS67y8WJezOWKCh3lRK9c1Aacae0WQvNIWW0MY9i1f2VWidaPlXVHLK%2BQLAiiQtvmoKb3vWYw1lJ%2BNMjTfDJoH4wEIFtStVVgWhn7PDzNGC87m%2FWuxnwSHFRU7iKCMWyvZ%2BKXXHjHSY8qnsPn8m7Ki4qLYX6bvTCBpisNAwSTCDmrLGBjqkASeFHwdqnamOfInDrCyYQUHCyJBAl5009GZxCGW8UJiIg7KqOXB77GIgio8chhNbTpGEggRHAAgtzvNRcfhBvJ8dNELllrndmgN883b7qskYETIgHMIzcJMJdZ0nqviuKyFHfKYITXChsslsk9lNI%2FnwNEE%2BP5Z2vBClXaPakpLQTbgkBX5CsgnSCPMJ%2F25DnfNOYK5BHZIJV3Bn3IyycK3%2Fw4wW&X-Amz-Signature=330667785d78fb55c193c6e5ae3ff1881d3fb0de0b9255aead6d6629965585c6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

