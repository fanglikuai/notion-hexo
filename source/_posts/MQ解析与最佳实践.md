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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZLMZLRUF%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T030046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJIMEYCIQCEhsMQ0xbTeN3iHjLvJMs%2BRVBxdhCUvuIRjEr6QwUYawIhANK8VjEUM2wsRR31ygRPfeZFSwMW5oivxnq1NGxVwzQ7Kv8DCDEQABoMNjM3NDIzMTgzODA1Igx4p3k%2BLFg%2Bw6X3yNgq3APLBZoSaAYlwlIYWWR6pBABricNjKO%2BGD3KdHcc1xagi6hegivPE93UhKRM01Yt52VQF6VtE3AEaHzzEgFBBlRM6A3%2Bqle%2F%2FgqS3URVxHWa3OP0W8Gq7Zg%2FwTjCZ%2BOVssB04WiGE644wLvTqX%2FnqGo4%2FKeF75nTFQiGvyX0fp9jukv49FzFM83XC5YG5hI07pd%2FTDr3n5hUKr8hQ5c4SYdKXRXziIZ6LFYyCjVi1K2sQdZsguNXeCg3W%2FTjPMZGDe2HVKZ%2F6yyEoXvW4vpwI5mMlrqNDGh0iIxaFi%2BnDRleLIrUXTbBpqlzYQKROWNt7hLcer6wXgnjfoox5xuIClDBSH40qX4CzeoHAc%2FEWudu5w0kzBhNfXqo5RlHfbP52IqJYf6QGAowE0JOqIeIEFbXbrLNR4O2fd%2F%2Fm%2Fz7VTjp5nJT8fhHpZLrnxIE7UssVRRri1khxa%2BvaqnCyOaoMgr%2F%2BLFu9is1JqxKMcZzBmQ%2Bb92AqdvfWlsaAaHQOKHuu%2BeDkqzuzmReXvF9WoyvMWeMvfKuErEi%2FhpDfGDxoDpdTtkAC6GfzKhpEGpfMKit%2BRr%2FodeFm1Bxz8EwJ9I%2FyN0pIM1ex5lUSenD3NNXZ2kTo5YpdI%2BxQV5GC5VeYTC6n4nJBjqkAaAF7KyaigTdXQxe8S5PREPdV47fZFE8HyRLYUT6PiTPEo3MrifkpJcFK2AjVGCjyDBBAs%2FFl4Mx92rStgfOfKp%2B9xsEvQsTz7XBs%2BuoOTMuSZvSsg9Xtt5o571zMtY0wK8Gd%2FwGO%2F8a3ycBeayidjGINQtPa7xyaLSHALbOVRsjeKlVx2mf4%2FkLI%2FIaj7fbcMnLbicT0CxZRs%2F%2BDw2pv3mwt21C&X-Amz-Signature=eaab99d1bfaef69637e5e14c066b5ad0e802de832214d505d528b2b16b3d56b1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

