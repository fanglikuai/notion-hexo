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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466URP2V7W4%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T010047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC28MriqqGkpa6fIoHqPFjbylvHSqFNVx9%2BBEXjXonGYAiATE7tK0qhzdi0jePP1IFovxvIC3aKLVRkZScazz%2BmJfCqIBAiZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdsEP7a%2FutS64TAF3KtwDvdBgycfqwaexCLdhJDLhnJmlfKEkWJquMnals43dhukhtZDbjkmNvi4dGjFkzW4uRzPvF7SoEjZy%2F%2Bin%2FaN0glnaZmW5lBU2j1GLMLmfcbLkH1CWu5z%2F65HdkiMXUCZtqJk3b2rjq%2Ftw%2FmrrZX0Ni3jwYIpjRr%2B4x4uPY45WaFcvFhUexRYYZA6heQN1OBN6T8G35oL%2FkCg7lhGgCjhnq84Ln1phhNZyRjiycx3kqeOJevpS4ZLUxirQpIBsPnf9MTBBZC3oK23%2BpvcF1ov43eSEZI0LFk0fWU%2B5o8BHRmkMtcaLMBTM48EzCpqDr1ywVas0fpN099V61dtXMx4ZVh6ZlRq2e7ckD7iWvtS%2FdEADcsofHjAOXNMm8t3j2Y%2FoizKj1fNWqV2c7yNOMlJasGs3SeuKQpPZk9PUNOjllakckNkLF0imIYizHGag8qfw86yz4FpqO6dAgc2%2FfC7D1IwPIcrhWSgLTn%2FdIlNU0mhNUChtvA%2BcxvjfOOmMerUsQAGsFmHuzb9uduaMDs9h0%2BZN5EwTjj%2FiDuO1NU3EfKDre1TOpzPYtSydz8O%2B3u3CcJ3maXNZFMhl%2FXPBItsozbhhTa%2Bc0cARrTqymtK8Uwk5GK99i9IxHXFeqiUws%2Ff6xwY6pgFawQYAksOQEdIXXgkGmo%2B36Y52pR9qs7u90BZqMK6KK58l%2B9OAsZ2un1hDfNn6vJIFml8emTZOJrTvRaZGFmvOWgllakurYyIL1XfwH2EGmcvT2KZ0%2F5Vt8KmYlVDQbLb6f%2B%2BJ1cy89AY%2FV5g6uwlWXXPauvdOMq4v%2F1FG4DXJXIJEJ2mnv5klswayltBdq0XQdSI5tPGM7VufvFgfw8YvycgBoq73&X-Amz-Signature=4a49140e6edba4e6c656eca89b684938efb81896cca8231c84c61c872b01f8b7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

