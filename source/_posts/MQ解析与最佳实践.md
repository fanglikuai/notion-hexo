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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WVPC7KLB%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC9zGmgE5LUUTy2pbWisTFJzEJ7HZD%2FbRNPkcBHjDLChQIgUEsAet5%2BIxtFBPBq%2FcwHhWCqRgWjHI%2FwO9KXFMD9D98qiAQIpP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNmBSSWj0a59XupbkCrcAwjTFvJJOwhB%2FRZ18i0Sl9Jc1j%2FaFY5o%2F9ifiS4eOwG5jVOIV2Xpc3LPwqEGTO28THWCaWq7e73TxN7MpgnhT6KVlH%2F3%2FgGjlRqfGUdAIWhVSB%2BBgCRvZJ4490KdXDtxhFAo8T2yLrx5AY9tqutRkySrmJxea6vwy%2BKU1MGCLIwez6zrVYQ8S2UGUPD%2FmwwP5hhXLDQEqQgL2bC6zOWgo3uu55H8D3N9sZMCns%2FMJGGqjP6vbLcVMBaiiARJE3JXv4rDw%2BaNF%2FBIm05W1gwSMKPzU2o1ch0%2FuvhS8J4l%2BwnAJZjssiQOyfP5WTFezYkmg7hmX4ZXXCUHhBlzCZI5L%2BA%2F4DZpfrfJTiVmAM0yGKIXABccgfHEVZ%2FIb2tZ%2Bvkf7yqHfneLtIQxIPp3zMaVKwscTZ6FamZxvseXXWNjrOhXHKOYrdA6fw5i8dtmoPor9MlrXIartULufrKkDqJz%2Bym5Nz%2BY3w7rs4FNvbje4b6VaDyxicmTRRU9z%2B9ALM0LoOLw71CiN%2BDv1c2qxjKlfk7oNnYK4jJpIsmaB8S5tE9lKsVj1w9Pi5FLjYjH8Hg%2FKJr5xU6wDzJazR4VqeM72XM4ddFcMkvAOXJe7OljXKHMJjloCd5X5BmHKcxbMPXEoskGOqUBfGymfTUBqc0JF%2FbpvQU8XixXex%2FQ9ytskO2vdDwJtnphYuN%2BwtR6scLDOQt%2F74GFcB%2Bj4wu5G4H9YOqpZebguzsoiNL5kZDZiTGPWLuVPmSbTQKypNjKNLtfspx0NFZuYfH5xL4m5DN5YzgS5AhKAVKqGKfky9MfO10ZvV1hwWlLl70MgiL5iZIkVN8B4EZkSRm4pOiQwOa%2BzcwCiCczrMktdL2L&X-Amz-Signature=58431e95916c291ae28814adff43cd65e1d6754e61ed5866877afcde9608ebf5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

