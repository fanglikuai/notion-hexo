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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VB7WXFJE%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T120039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHyVquGJuqat2HRFOlxKK6ufWq%2FjmVOcCAh7QuvsiT2%2FAiBYbkbqkgMuzqPytFLSkjzAaE3tcu5nal9GbnzbecpjoCqIBAiN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMcT4YoR%2F9FuM5iqOSKtwDAL%2Bjc%2Bn8I3JKrtlEB6JafKXImKKWt8VH2AxfjnZ2SdHEie7FEPyr74HjDhYSwDs%2FD2kPjGvYamdCcDJCnjlHIdeHj1w91cZm5mTcv1ZtiOLflSksxgpW7aBudeQ1X%2B8potMZt94o0KfDDwryWUSRvCRV1C0m43c5wPXE3BCzsRvI7up4D3kemrin6Y8qdsvZtlRRxzTAWz00UCeKwtwOFokqLaUJqyNo2KAtMG%2FLGQb%2FhxPRwC7ryLiHAsAVpiGFT6SgkO%2Fudda%2F8vpBXa5PErf0zLd4TMy%2BWV%2BJtFihy07o2qtTwOd0TXay%2FJmCdAHoKX58k32ALUickNESxVMNgfj9SFYMHE%2BOVlbPMc0gnwRSjCwH4yyz0ngHJrsw775xPhB%2BnsLB30K4ERnNszjBK%2FMC3dthsNX92LAW%2F1MHyaqMi5t%2FeZb1ffHxByyytC6h5jAOokRSV4TQgSBPvF8jPUPogbWtAgoGG24JqeLgarUZE0dEwiqwJ4Nj6svjsXERHM2%2Fl%2FqutNsBAxCy7sstYNJpEoUChDP19NspeWpn%2FzlW6Mvn6CwthLc10l5G%2BsZVW7ev8Sd5k97LhrwORNcgjK3KH43It5Wi1r1qPL8g6vp%2B8qnut9IBGCsOeX0wpe6syAY6pgEiz4dedaiKa5yE5YbuYQ3ncxMkWZIzl%2BWkGh%2BLiOA0a6HvjrJJ0oLYRjJalUYKNhFTN8lYVaPKfXOtwtU3x%2B2kjA0Pg6VgRsNkFHzH59T3xhEurdG%2B9IZfL5N0mHbeIapB89PnrWC6YrWzgjqny2u3GNGvKvKk5x1T7EEOe29nGmGe8RxgjEejJtNvYNWyDmfRuao09acbulvSbuQVjisgZmDDgub2&X-Amz-Signature=62516e1fbbf269a7a4387e2255e0ea8050d414907d011a30fc888b9cd4c58db2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

