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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46622XU7OF6%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T020039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDoGS22BoBFajOXthKI7VzKs3YXKnhk7UMgkKHSTfxc5gIhAPcaChQ1f5uIdT2yMg1dMFCBPwP1hMhO1gSB3EWrivFbKv8DCFEQABoMNjM3NDIzMTgzODA1Igy0kpMDIndqvC6uU%2Fwq3ANSMcw0G0mWYauzql40%2FbHdndEU%2BQd9CQExyV4p6tLnhRgIs9KuhYrWBalkgN9FNSkpLsODcFYRMrjQUd3%2BA34HXYFyQJnBOpc4WKdYadCxVBD0V%2FA3j%2FtDWt4a8vTbD%2FZ90PFICMCNeIYhuJOonUixgEQCpeJzVIHlwMMhjIURdwZGl0GUzj1X4KplO7iJVajWA57%2FAAA%2BYCoo0lJbQTYzbXVDZXs7FDd2RWuK6NZvvnEKwMW9Ew287lH4xlBL9THM9ZTkyoztgdRWehzkn7F%2BB%2B5tNvCJIR%2FofUi2dY%2FzXTdQmsNhmMGAf3szED2j%2B2BvkRl%2BZyemUrIPIVxJ8B54OrokXOqHWmnMN4gfQPnkIevhO6f7NYun2Xs4k5xEZXzxh0topxKvJUyCDzbUF4rLwfHVfaqrYklNRW%2F%2Fg0wwZr98mSOwbb1aoJyQVSNw8qfxGFUq9YFVBqkIE0akGVfgAeZ5iGuuv8chXQAlwy0a1tqHYn2gHV17b2JuX%2BGVAW5FoD1%2BkOueDb%2BTqljrxP8krxfCy2u9WxJO%2FIT5YiSxJNA3caUqpzdLfAt1W1ITIplvGNqxW0lKkT2nLJI9XIUIQsDufHQFnCL%2F20yRtJZLi5JrcXGvJcTY0E68TDCS0IHHBjqkAVV3jlpp7wjbc08wDn76rkBPDYYh1REw1RGnG%2B5GHdDJrsmM6ZKdt3wDNY3AtLopwh0GBw4%2FdWKJAJGXr%2Fv02d14N0ml%2Fbp3SfEAmZGMSSHwYfG1alHn3u7yzzBt6ZjM92UNAH0XyEWlSpoGLsmVzDNZYkplCyp6aHuO3LBEolgJmzJibfijdTl6j5O733mS%2BECuzYIT%2FNCnnqsbtHwvxTJmhM1Q&X-Amz-Signature=a4ab4758c119ec9c89916787aed523969fc171a4584f4700e08c2bdafb8a8139&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

