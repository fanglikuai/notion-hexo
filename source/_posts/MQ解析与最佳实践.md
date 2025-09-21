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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QKXUEZOS%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T230043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCSuBuAq%2B1y4uoksjUsXT0UHPI00XLExRB13k1Hfa6R%2FQIgZpmmH4hIUIF4SVuGbrV4jEpBeEWmUnlIdKpz1TzC%2FTwq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDGvbTTYYcwOQGtjodircA4Ps3CgFNJTiTc5O4mDhLD7TwKzJcfdrYf3x6gMKwsLci7dEp9riLWSFd78jAu6AkR8ZsaV9ok2XQt7W5Mo4A5%2Fh3PTlfe9oEpN0eHr7ySl0r4OjVy%2F30g1My9%2Fw7hKSHw1eg3NDx8sU5B21Ih7ihSxq4YjXvihR%2FiIjDSDKb5%2B7WOhNdv0LhZ3INpsNWppkXTXC0WQNiMYcO18lVxrbviUcSaJPXkOLzcZuJNDYOmyFLXgVl2yDvLUU1FNQ4A9HulqbVFhS7NhjrpUUgzW6PpD%2Bpd7%2Ff5uiccf%2F0qJkiKLP6aqd3aB%2BCTa2N03tVF0i6kd0%2BRMFjRYnOQ%2Bd1bJwd2R9n2DF6oPxSIJKVVq7RwcqXcLJyhFfkuTpDvlV%2Fw7iFWMVeTttI8E30bGRqTk%2B%2FMqF2Z5Ahw5lU35luS4Mdw3lwTkKFqrrswF5qngyuF8vS5Wab0mB9YwVCEpn5Pi0oFm1JlJsNRN%2F7%2BFTNUVgPBhjUkpKbUVUryylg4%2FOl%2FpuoZ9zUOiQgt6wQ%2BRVs5Ld0nfLt5S6z4Fm5xS4%2FZ47HAUwbzJP2wxTznjjimJipijYKoRdYouguEPYIZTuUgndckRwJguDpxeeKFrgRFw08BjsP%2BwYGDeEBQ8zhYrKMI3gwcYGOqUBgimtMW1v7eos0SfP8%2BAA6mzW4qvihKWghtEZd4nzkj3WKX%2BW%2B8kXvBJOIixyChH%2BgUoBDObhkzku5G6cn%2BrbLkJByRUUckjwcYiAsPkloGn8ZhFu1HBmojx5qzLZCqwyji%2FFLGAU4tKI5ZJDUf9342rywFg8BYC0mnUnkg8ZJZppWSLVMBQwHFD9OYlqcVP1fwh4ShGqXfQm3r1d5o5dckqqx8wy&X-Amz-Signature=8ef728b9e43cf8e158f43c6b51d69621dbe8776736162a0729b3f20567e13fea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

