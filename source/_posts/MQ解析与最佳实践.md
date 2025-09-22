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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UWGV3OFJ%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T040042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCSyF4ZtSH2%2FQuRDOtNiygXtQCagiYPQGuCTjIYhkDkcgIgSbhYLI%2F2AOl%2B32c%2FOP4RHGLZ3JP2YHzsGE4%2Fo98OOEEq%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDPtYD4E5b5Zkm0Yr%2FircAxoXlB5COHrbZr%2F073gmXVYtWYz8ckqusx1xpa43VbXnB9jc4MwaTVoeEn%2B%2F6J2%2BDLE7iEqAUSoxrzVFqlUStEW3qb%2FxkisiRSq908qzbiM4ycihIc5nxUvrtjGOZ1l0Q28gbxfesT5YP5Z4wRPX%2FTKPssq9KWQIhLGWlgSokeM3uCU9H%2BAz2OD%2FJfbSD6rIeSjpDfaprzw%2BOvrI0K3uDrd0PiMx4i088uTZl8xUb0311VdZQnqaXJ19pZf9uPgCPjnat4UclVl%2FtHaU%2BrBzh9lM6Yyvvf1KyOxR2tyTVE0x5YowifpnpCKsKjHZDNALHuNguAHcouHE6zADM%2BIesJq%2BwENE3JTuZa8RZwotcNr7Ph7YTRnSdifWZ9a2BSYIvSfeEN0yolStHlLOoIsqmiEGMKCxFeuWrmHBEQgtxsEWjw2gYXvLMI%2B4R2UDiJYf9T5ZpzBpgVSNnuJzQ%2FcXPjRrcHAJq%2BhWF1bwyBuWWoYr6GxO0GmC68phtqbZUqG8I926L8k2HIFYYqxBD4G5v%2F6wMiwhWF3hGUbN6Q0kQrNEdqqjkFRvuUL%2Fow1hQ%2F3tYJZkwTqDU3UtMFnSTooF5dOg3BEzO1%2BWHd%2F89GzIH7gkrh5NRB6VnZPV6O24MK%2BwwsYGOqUBLCQZ9Wa5rEQq4qHxgP5cHpeQx3Wy3so6hFCEEKWyPvQXCEBPi74%2BAOa2gPJVglG5xWd3Kbxgc3SudH1phC3o23cok9QBtQUJl%2Fd5xvTRVYWtVZBi4%2BkiRfjtY3E0C5iIgkfpC8ULShEmhNMJqIB%2B6d9hbR95XnE5ecIwxggQs8elZN1%2BdHlzlwJZ8NiXwEm7PfhN%2F5ef5T2esSw7ZWZfHcyA0USW&X-Amz-Signature=9b4ce81c5147623a684d1f336ee475383722930c279666d9a3e81801b6c8ae91&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

