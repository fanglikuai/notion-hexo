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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UVIBCONO%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD5fRxR6W8WnJ29gVgLqiGBnDz2u3gDg37hzcdJ%2B73S5wIgA8p7YUt9iHWFa9%2F%2ByatbRjvPkFqes%2BJY8XAID5UqE60q%2FwMIThAAGgw2Mzc0MjMxODM4MDUiDEB3RSnDhKA5mAAZTircA8mcSfIdEV8dGhGFcRAQ8p6%2FysZ8W92wpJBTEHJFqnbjtQBgrWbZHhNlnKjWuaLQCATxDcbCRiyd3kwOwfzyFNhl6%2FVIxRcyj7m3XjAh%2FJXdoxbASotJ98bBwptHzzDGxJXl53gXBW0CVFe%2F9enIIwWfXjIwCwX%2BiVbcnsA3dErj7gjhVn2UtISoAJ0J0B9leGYXf0NkbSyV9qAQNa3LvWyQTnBGWJqTBbzZRNbEm5lGkPb7LjfBabl45umNS0xFE5TO%2BrMTxoEnzWngb6Q%2BvZg3vs9PQ2Cz3BUxJ6n6ZzXe7QWMig4eDW1StDC0ZuWLTka%2BXikjJjyp2FzaY2PpUlWoB8047c%2Bu7qf789d40npFtXXnJirE2xrcbaN3ZBbIej9kL6lIRIOK7rVxn1AqIJLjrm838XzFjeX8qvxhJ5MEkJqNQXaiI9y%2F07Ysvbueft%2FpazS27AaractmvKkH19CQKEls5akYIg5P5NsTAyl6VgAXVntgH7PVIJA%2FjJnJXu5pfyWSHh8e8j3FDBPUqx0%2BD5wbuzJ6Z5p84UgUJam8fA%2BfqvM0xCmIZzuTs0hvRh%2FP4tftuWAGKNT7Nxa5uovPbJQoANWkVrwkr9dDaOSZNzV3pYvffpkTDKTQMIPTtccGOqUBvncuzo9xCWahR2qQvUfsZXTCmnvtRXpb92CwrHjM775xr2E%2F4IrW03JOr9AsHFFFZMu9q9ncIF61Ugy4kHg%2BY8T7hf2tdxamds4SzMY%2FxImrILednshb0U9speh2HnZaxnEW03cc%2Bdho5J7AZd%2B0U5padQZTtB6gBk60gn6hlyLNFTX5EyFfWbhEr6gMUKNtYJ9uv68ba%2FLPWY5z3wYLB4otQ5FF&X-Amz-Signature=7cc83630fbd473817a4805c5ebca5ab67a8500c2941afbd05d9f2170e1ea7786&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

