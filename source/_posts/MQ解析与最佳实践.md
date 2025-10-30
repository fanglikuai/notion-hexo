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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQKEIUZN%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T160205Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJHMEUCIQDjeM2dEJcAElGoK8aULNehqnz99qSribvbfj0rPZyXjAIgAoLjx44y8JvcGQl8v6E%2BJBJCd69tuUxHeM319F94IDoqiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFtZWu2oGl9NxikW8ircA33MvCMWvyPesI5Hq4G68psBcCJZiYnyY8QUlcSS4x3FGAYf7l25I4AS4WnyoLRXIkV3pjMhal5zTrPOQ3siaadDqDs%2FiWB0FnstXzpFmol5juQgl1ZvjEZeq2%2BoYD7vlBm6jwSAhPkh1%2B%2FW8fQhqw1pLGLyKaOYLD2gLjf9rBJgnqwNbRiqd1TrG%2FCO%2BR2%2Fz9XX5HGmEQMuw1kGSsKwqZWmF%2FlFJnyR%2Fc6Hw80GdMWoZvqiUADXRlgSzC7CpK92MS8rCaI2jjWska%2F8DTwN05gk%2FdJEmYhC6fzkDrX3SD%2B9RtC3pqkqLnhEYmLaoHPqCTBupDQtEreIrYDFfDitWKd6%2F41sr9nl0%2F3S2sf7Cyk8%2F7Y4Pcd8oM5IiG8DBvmtxZppF5Y1ndbrMbm4GudKJob3Oet%2FeHZiV5Tt32qiJJXIqSMJC1nXY5rQDcMQfOQMGk3VwaFj3JEh0rejU4ux5460PrT%2B2YzDQ2x2KUGuoiZjHSpKWuvVTMv1J0mI7jJg4H18YiNBtcHndhV0HgOlW8tucoW112oa4mF5y%2BYMFaU7n4AB5nLgdrGp2TeCIqXJJqb0P8n16kxrnLRHTAFeNDQuzg2qmY6lXt%2FGe8BTcnJeFR2HRVbMczqhoY%2F6MLv9jcgGOqUBxDvkQsmY9Mm4URlcW%2BFcn%2F2TttUiHGx35YGufuZ%2B2P%2BNJDN5LUimiUldejS6YFuAa0CmF3tekYYy0B3qeUSr0dNMiBA5%2BjPhT8vBvHY0j0Ybm7WMZ6LJymiZVgCtIETqqu3X0cQ07pLYj8SKoc2PuPaZeKQtAMkrkDEus7sP9IyXp7nqYIFZo8iIi4wH2MnhYkidkHlpeNEDYw10WSMaw4HfA7Uv&X-Amz-Signature=aa1b0cc2fe408d1b805eefff82ffb19c5a3c4aa42a12335e92fb8be2a79a9913&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

