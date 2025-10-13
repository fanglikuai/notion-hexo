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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665TXFFI3L%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC%2Fui%2BKe2uKYFgl9HA%2BRPAj6BL1YRr8ucwgi3kgwB0iIgIgFkeVxBrEufx8axfRkS1IlnP4OVOZnKWsQ0vrjsY0Fccq%2FwMIPBAAGgw2Mzc0MjMxODM4MDUiDLFHUxAXV%2F0goo6b1ircA%2FvnrE1HALh0UnMsuRAxfH5HDDPJNgQ03ORrqTZP%2FaLXR1AWBLFKMjBcRnMwbHRi%2FJkWlct4NliiSK3GvtPA%2Fc7ZfsDl21uovOSrI%2Bju97q36VjKCOKjm86L%2FHpcL37%2FNImJrMOGe7iuB9nHxv0k4IxVQMmD82XIg4ei8qqF1rxp3ZJ7HBgjgZGrv2oTJgSdtoir0Ebk59J60GWT%2FyAKZ1rclsWQjmHftDI6PyNo5u%2FjLd22hMbFv8wTHAecvL9bppXl7i312UtXfhnQJbdj3b0EYDkFTz5F1U5EQr21KRAGTtUu%2FEzKByktNi5FaHuGZnV%2BkIHehcVzZv%2BrDL%2BaNfabH9AKvESUYUZDjHmONAlFrenxTGs0ZoJawJ%2FG%2BzPpHgz2XotZodKQm9Cyb3uyiQCjLvJv6yxUp3ov1SMNULPHDai0xu5kj3vE756dhfzJycv9wnRVPJAh6IcFMydRwNbmaiQZgOUm2rell8PtbAEBZ32bjPRspyl%2Fn3K8aQvsqiCtLGb6Cv5RbVxnNfOd1BsXot2jQfCIiYF7AmqOdIjHQpBwUtHl3kdXtTPGitJbQbnBanLEXt%2FD3h6%2FxU4IvvLohk0EVSFRDO2TLIMONR8lZSJ8PZ3Xcu4vw44eMKzUsccGOqUB47GnWbJzA0J4YqeKA1ZORLoQQI7pviZVA0KTdvq2OW2aG%2BGOgfD69x66QVk%2F9CxZB1TcWcDeJbDxAXe9PcQ%2FSCjoSSxIOY%2B0uCE7YwdqK%2FZJdUT%2FCTWbLVmXA9lH7kE%2FWJlH8ZmgqdLeo1poM13wz4TK6KFi0WRqhJ5BE2ptPdtpL36uwtwxjNnLCfo%2BsjxiG6GQj2RSBT5F9PV4kmYnCXYl2%2BM9&X-Amz-Signature=527e039fda76f6235df068694c1c343ff580b3a247ad480c0b5107439c2a8710&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

