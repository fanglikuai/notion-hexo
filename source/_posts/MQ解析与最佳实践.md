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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667DJ2JLY7%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T190042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJIMEYCIQCWOvtfJ%2BxuREM2h0i4nQWGsBBkYlYaD0no2JvDJMM4wQIhAOrzriEiFH7pix%2B7f%2BC6o7kGox24pjhssvJT2uF0SXD%2BKogECJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzwHIvkf%2BuX7xjo0vEq3ANMUfJFkRK3fWdWCp1QgQv6tfCUWXa19nxmlwYWQuX6zKeMn%2FqcblnZ%2BX87ZP8cazjo1lazLpTwzJVbPigpcYHvF49yQ%2BKSqht%2FXz6BwDeicgpJZn7u2zyRsyO%2FQEyjc4t8ud1BFQUN0Hww4KacHFFi6CauBQfTvD%2FxW2suN4XlNKe41eDz83QujotfGL6Hy7dXvUVaCnISmrjULBy1rI%2FihXlxX4g7x2PZ9USHubHG7Jzji0vJOVXtmMm5S%2FjswtvA6eJ%2FxZ%2B1cDWZ8c3H9b6vBHpFqa9gdiYLGM3GKI%2BVgCbTpX00y5NJYN65oN3Blt%2BFHUhS8sRK%2BbegXFZlHr7pjlm7syBEO05AOomk%2B59QFjR%2BJ43FsVMOIG2zo%2FTh%2BJupqwb9LP3%2B%2BxiYZpX7C4fLjGGtLJqYN0GYlpvYWRDKsZeu4vdKfbhLv4%2FzgOFLPdcbDnl0sCb6ok1J58E%2B%2BcLxswIt9%2B5tJqaDFnN%2BTII7OWjq%2BN%2BYyfAna9UTEF4kmybWuGtIIQoys%2F02dZMknkgB3LI25RXxpVQesSSXQkl7ZSqxHEHSvjtOxivqFfS7kKTvjddVBmz5L62fVT5WK%2BMcdam%2F%2FWjtT1v6PkfIRppzeCwNiGiHQjKyaqCcojDS46bGBjqkAToRxeDloxwO2KVOxBXoJYnHrLi5cm4FSNHA8FWUNCmGAiRrLgngjIon5RR91y1wtQkIJMi%2FZHccErp7TsHbMdhJc0r%2Bm5AcHVe8N43vHHpt0Y37g80cKwY2ZdmIOtUXmtZF%2FmBWw5tYBPwRk6sBuzQLEqKqdpoew946O%2FOGbRVGjgMm7RssoEGRPOpTM91wbC40ChL3VlcOG06vcExAqSw3k6dN&X-Amz-Signature=4d3e393262a693555186d027ca4b39e4ffe7f82542ebfe292ff2d3970c0892fa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

