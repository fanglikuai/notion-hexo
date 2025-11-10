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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WBGN3DOI%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJIMEYCIQDLxSzgAdeSCJStTMx4BQXcWQIpII%2BxJJ5DdFqYxUy%2BsAIhAOSKMNA0JMFm4EMZzZcp%2BzErwwp2kquwD5Uh%2BHDGS1aUKv8DCAIQABoMNjM3NDIzMTgzODA1Igx2qX30h96Wd%2BaAsHUq3APGUq6aiEJsiTcFI7%2FEYhYi0TV5CPblZpF1bWbL9jEgUEavh9zHlaPbe%2B3gsV4v1T7%2BFZzOr13bNyk65v4FXfNLj%2FDARxWh9Dhey%2BSuvA3OwuC3ymbRUWsLqRX9p2ba0D76MaY2wMUB0BSLxhGCFscx8jlWdLM%2FI3Mb7U4ZmfJXebMQp3Jxlu%2FhoEELzSkjOPpN%2Fuj28z846rqev%2B%2BhA82DNowXggr2SXL6qqNYwTk5Tge7dCoV%2BqCUuaP7m20DG%2BDnFDYn%2B6PAzjpCHbx%2ByLSY4Kwc9Fuu0G%2BkAxBig9WKRNnbA%2FZfUPX1uzAj6zrBOpzHmajlkH35n%2BQtbvtZvjoOqH1qmaX2zQa%2B1ZZYLv4edag5AN1o%2BIyZ%2FCGrt9FVBqk2yG7zKacTn8TFvrkmZ48VHhKmJS1T1ex6vGo5CCDy1XYjE1yadgbMzf7fu9zHGIzLORp4dGbC6GpTdi6FfeQpV2sw8aIOmVaRHHGdCpdb%2BIL1Rv4GWgC9M1r61f8Kg9pCqGHfaWNP%2FVxBcnx1Rq5YjgzJw%2Bgwxrjnl88bZe1lkyE49brIwYM%2F8FWvae6y92MdsvLa77XY%2B3gVT3B0xzG9CXtJzhG6SjDEgaZtKK3TR%2BNzcX4M5uoNHeX6XjDY1sbIBjqkAfbuIASCcxsRHGuCztfVEKnE4Xb9HX6iimsHEXPmJ%2BZjYX043ALX2ekkUlyMX%2FkVm7UHelUgVWTLNR6Yibx1iZNDKROQ8oDCpocMfmqjo2su2pLTjnDcVTACnp4BY7KzeUJ8w5kGNmXiCYDoWC6lScQLSMb2vsH9STMAlEM3JOVItKETzQEwmmJva7uNUzqXMR02jXrhh6nc6EoYc4m5HTO2GUop&X-Amz-Signature=1a9be751f882c858ca0dbb9c3f9407aa0ec1e92c0317282ec452521b4053c57b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

