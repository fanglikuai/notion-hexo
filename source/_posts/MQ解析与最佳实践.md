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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667PZPHIXY%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T170119Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcPCZ3oySTOP9ZTHAte%2BRsKhlszUi2pwUuv45v40R6SQIgR%2BNn2b%2FccTdR5GQUv9ELqPanwOYpvMvsbn8xjzBM4uIq%2FwMIYhAAGgw2Mzc0MjMxODM4MDUiDNXXTHvFTcsbiaxC3ircA612EMTD7JjiHgINAJH9SRnJPAKyxwABSSDhQ0oMr8t9C3NDM%2FzKJTgasj9ywpsiNEBefnFO9tkWdgwFp%2BqBJkBazaIGbo940iOeMueXNqwnQjL%2FHQ8B7nlQ0ch51D%2BMzSIuWWrnBnCXwAEWawner4DXjy52tdfDlDC5DqPxMyr9447wX5Z5QJb1%2Be%2BUraxmJTY%2Bw6rzHr1YAYhqe0HqjxxUyxeV%2FACQxCT%2B8cIuGLw8TvL47pKxeQt673y1%2BjcFlen5%2BJBLZl%2BRKKhGBGN1sI4K8gDvRr2y%2Fw1d5NlGvd43vjtHLmiTWcvGzRP2HSpl9GW5Yqx9p1nc5TJYPtwMIvNBzcmBOfC%2FREUirXlzM7T%2BJWyrgj%2Fh5xyxg3sBYcN03oFLfME8XPjPZBJGLPiI0VekG6I%2F4IJdsmyjLj9PBcA5OtoiAakB3ZIWtZnNqVCN2AIOIZ9a2j%2BvBrKEKBLXbkr03n7eW7KTD2A%2BEUxY3daR7W1vy%2BwDoYiuklQCbNQXzfyACZfUJYPSvrDmTJWoUbNwBxacc%2BGwq%2BLs%2BCRGtX54jebPx1vuppBZ%2FmeakVgBhMf5w%2F6MwcbHxk%2Bvt7UWnwBJ5ZLA2Ci22X1Lw8%2Bg7HhLA6%2BcUNr6v2JjhG%2FKMMr3uccGOqUBRs5K%2BEM%2F8mwhuRl5WhKmbDbz4VWO8KR4WKeuwnScsVTZMjDBAM6AHvxK4Aukv6oOdTvBYlm67Pw5hwa2nSL9OYz4BY64DCVmhItK11DBkyXvJtICxFj8%2FjuM%2FkQUg7HlFkPbg7NWlQu7HwCrjj7ar7lBfmW7%2BDicPwVGj2zs8o4aMujWFlQ3h0vEH4RBgi6ulGxmvy1Msf%2B%2FvNziq3pBsVxBkKnK&X-Amz-Signature=dcc48dbb99e65b0c3319f4f598381c4c27fad8cc3a636ca3a8e08fd3394c0413&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

