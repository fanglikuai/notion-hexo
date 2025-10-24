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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466THJZCJO5%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T170055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQChdtgX4iZHi2PQHRAzLCfOPjEfWjTBSB4rZ0E7pyVS1QIgFs1Y4DWso%2FXZ5qGWRsOOnHW8nyDars7822D3%2B19MPtUq%2FwMIYRAAGgw2Mzc0MjMxODM4MDUiDJNSmDbWjvgAGJKTWyrcAy6RljDyOSd7XTL0TMBpsVQrPJ0uoQVJa4LjjNe36BPelNVuKk%2BgjhYmvLm3k4rqkENU%2Bqri5j1SgLi%2F6kdp9W2Xi6UgHQn%2F0cmCPDPeJ1QLNvQ3JgcurXJ9lW8lZf4PfOYL5fwikAnlKwh7ksBhoH7u2PX1yQqMbiVONYltVxJoG4sbtwcvmzjDLY7OVjCpr5Lq8wL9zCSzyksF%2BnkVQ6h6uPY745zlvpDk5NGuUM5wmy%2BmEEelNnxppENlRrGXlcQVbST%2BkfM1o%2F8Fbtk0yahGtYaDdO6uFca%2FhDeucrZo36hnzKCTBBtNMSe8d9L2U6zIXJVOGsVMaianBgeQ7Fr63YorrFZLHv8BJwYT4VWR6NnzGCxZI6HvRLS4Ptnm5nObHK0F2wVncF%2BeaUf2efs4zsVZPMtnW3t8JGiFufPq85IpK4DL9bOSSXiij2QR%2FyZMgW9ODWDqKJhnzvw5hFxFUIhUQpGj4b2EWOcemJwgvEyCQ338C%2FkCQ5sajfVlVMjrLhHa%2F8%2B1cVeGZrW7%2BSXtokFvkQ01w%2Fp1greysCbg0J6%2BFVlXO5%2BOKaJRHYFnKiyIzXRgE9Mf3aXjJ0IZCpdL9v79rtTmJjcWZ3KJoT0J%2FZ8rtgXgi5OSGjJvMODN7scGOqUBMj0ObujOWuTDTphYbUpz9Xbju1ByKi0wThG2PT06guArWgBB0bSFPD5EG0QPxgCvqEQ5pVoWnu4htJmmtaH%2FRmehJmCvCYxfZ7od2VBIRtTgLYdxUonFGvreR0PxqjcQfqbS7rWeoLTf8jFOJN8nmL0WEWDmD04hb7tcWp3XzEqdlndBZSmlnBBMRY%2Bhts6tSL14LnxiVVw8mVmpAFRYFK6jh4L4&X-Amz-Signature=655515544b9624009305c22f2d3ffb98eeb14f8b36cf357d3bdb16d3dec3a88c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

