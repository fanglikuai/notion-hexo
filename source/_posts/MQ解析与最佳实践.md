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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XNLQNTN5%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T220037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGeOGFKs%2BGYEbv8zif0T6%2FbxLyYaVoMhcsrvBM9h23m9AiBAgFIYq3ZYGz3c0ekWdH5054A7g4GDrbgkOeSb9wal3CqIBAif%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbjd040Y9kDEQHHQYKtwDXl3OJq2VRovpEgfmC21G9XsAGEeqGxsVSruqATLMXqNtzimB28%2Bp7xiCBqqPxqBooFSPxERQEOGs40a7tJeZb1oIIr5Ub8Oj1T7LFlytURD0BmsAlUMMZhIMVofMWXOD2jMUPKtatOOo%2BDe9kCNhchs%2FzF7RCdgaSDw8i5zxILidCzQIu8e6CEagC5vCgUBExR6DqekdWzNSaFvySrlw4%2FAV2b0KHeugWH8ykigBVmOaO2vOnMqAWRL6ksOh6FIDzv%2BGX1XnjC8X4XwyaZTkd5FJrjSyYx5vBhJUstQXRIQsh%2BVDr8SVs9vzWA1ubX%2BU94%2BFm9IT3eyGMjyKVpPBdZ8GcrR4bY1YW9lR9la9u7%2FylEZkifJ5lzhmkfNNtrnedE%2FtX6hyNnSuHyK9QROLlV3uCPyOmKLT2g81FLL4lT3f4FdewvkJQoxTt1ROW917e70zXDYcyPp9i86PhJkZQV9Ll4%2BvtN9ksA1NysyOe0u3VA%2BPtTlweWccqKhOGoJqbyoX87XvlASBxmM%2FVMR3mIasvOBrzy9%2F8LR5nJxkPbKQ7Ogv4yGDwrdStjpps2o9HQt2RFAEFSu3K%2Fzq9x%2FG8bX%2BETWovLhy5OChqBGHJP1uXO0ao7kNvCZnHTQw0ZHpyAY6pgF4NYbzgYqZ%2FISo9I3kRiX8%2FSJzypn%2F19jxwyYCPGcIs8yQUeaFgnryuwFSqqelh0n%2BmeuVNCFznbH%2BwKvvtmrKVXhq%2BHxseXP4pRJT3YgF0gOvUxlCc2%2Fp54o7Y98a4JGE4SAt%2B38ebmbK0g3YzaZq8W2kioSX7CfyqKiVxc4vLgFVRpHZQbwKQTe%2BNsAg%2FCi1EXKYZBedsOxB4L4W1ZKHb8OY1yI3&X-Amz-Signature=dc4ba0e59993efe49d91a26f08ee25d1cd77939fad8379e396ff51ac0c4c319a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

