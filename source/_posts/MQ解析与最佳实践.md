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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YJS22WSP%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T230052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE4aCXVzLXdlc3QtMiJHMEUCIQCRakER4Z7M4MhdsfwPr%2FfcoqeOjruEU%2FfqDMJ3r34OHgIgBLwwAgmn6hxPgC8SAvyzkqEMl2yXUcHuA%2B6spnESVNAqiAQI9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMOvtZgwARLIBkvqZCrcA7htZn0Epp1r5JpDXGgvQpcoooRLsU%2BYHsdvNwX6Obi3WVJM%2BYtxp8EihHrJBe4psoo8uBPnWieW3lb%2B78EU2mDgUwuD%2Fv0M2DshBkeTbeOkG7QVwa9HtYbh9JmHdSozA5VhghdKg3rj%2FfBWYJN6XQqgK6dAJ9TEO7y4ockD6XIuw%2BGkKyx3iZS0IFj93V%2F8ZKF98iJqVbptWPIicsLlXPcKs592UcW%2FLPhRFRq2jJe0SmxgE%2FXhyOTAq5%2B3FmigpnUDv2afWz6wpRIWN%2BXhY%2BP9iApkZeNwfMRvDgVtTjQqyG3MpsG1MPohTS6fldjx5Hn3RKaqhN7fPDbgGzn904KHXSubjzbHnquVcPVyy4jlKWs2sGp18l3AsuS0tc1tUhreikzwknv3yW1lx1T17mv5WKy102DbtySyO3NaKWGX6TQQtIjllXAiptYfk694pTXALjSkNcNtsLVo4S6mjA9BKFi3v0i6aZTHvKHTZt1%2BQYld9tnCxr%2B%2B8x2%2Flig%2Fyl3axb60iaNkg2GvJAehp09%2BMQ0tGxaO3UEFJGCjkIDppyigLyAlnjpRq90UDv9ETEpK8lGenEK2Wyphn%2B%2BNsp0Dq8wMS4rbxDYHkZ4Jp4GpAo9nMKIINpeWYeXkMPno2scGOqUBQgxwvl2rbOzirULFmrHTZE76rT1eEvfSnejYoPZ6YosaON%2Fl4kQF2rD3ui4qwbbLYFRVcRAOB4mGUGUoww5%2BKVuG%2BKcr6qhprk8aNyqeVkimFs3TfqaKG%2FeTxRhwzP0vg3sgdmPRHzypkPQZ5ZYD6s2Gpr5%2BKz6LSFsqUoLuoRZiRD%2BFRho3EcGr2ugRInce12gpikubq4x5qVTQJfuD5kXsdiMv&X-Amz-Signature=90f77cb5d039c4cc296ad02269dcd9ecb5db8405bb974952f79762e45fbc38dd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

