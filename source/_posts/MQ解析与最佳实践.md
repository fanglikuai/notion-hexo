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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RE5U4AD4%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T030043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAMaCXVzLXdlc3QtMiJHMEUCIGP5qHl7zNGVoU3KgmayMbDJxdFscBHHuGcifIY9%2F2NZAiEA6LgYBPVajtxCOijBe1NtcMbf0vIZ4Bwp3YX1cLB67fEqiAQIzP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOd2fafRwhYbiVjTmCrcA8VVB8eHaqj3EmeDIARIZXUuyGd9%2Fct1C%2BokcG2qbqNkZmkHIN8eeZj%2Btv3eztANYfnTl8Z%2F8%2Bz5RBCxITw04vVyDweR4t1TJQk8KCvTUk8nTlYAF9kzr6fMNAaE1eSp0wl7%2FZJ9SiGK%2FmEM6JOJFKBSYkwVJlAWGhe3m7jQhHbmdOxHdCfa0Wo8ZYgmm%2Fhu%2BoY4Q8KBPptRRIgDCdSEvq%2BljCxLSTuACOAiPUwNUTdGO9i4i3OvDlBQ%2FGJ9bYvzLmB4D8bnSq4i0pI5UwBsddPjAs7gF%2F4pTA5RDI3Z0VZqsXAoyE%2FCnHugh%2FlMoYzj336sEIpSGcxgTPFXxwtukoIkfQ%2BOaobdu5z1xI9fG%2Bag%2Bfs3i8k4HMX1d18LUSfoJ6bP1CkG0c72rWwgDCOaL3zQrzRTVZ5XsING%2Bov6Gdy3yBIVuOVVqTZph0iKLElOjBkLqeAP49dbj4LAm76Y1vEC201Ot6jUHBZVvxn62hWf1SZZyQyZ5SM%2BduRhA6A84QbCa5mTJFRBozzcbpuegvLTwE7AsBg%2B7WxptnerjERrpuPVXC04OvOL0EiYp2CNczK9qlxQvIaeRlxLswxblwAKNzvpkBxoKg5VTm7ecjnHzIhePVvkuzGe3aI3MI3ZusgGOqUBqTlAceQKaNCP2%2FDakTTYid3%2BmBaP1kPcr4ihDxx%2BZoGR89izMywpJCw%2FCVTpbYkV47vUH0x6gHP%2BJ%2FuTUdy0PAracMVjcDR5IuogZQ3xotGi5AsZFQcbrz2XkWURarWOLFDcH1gnKcCuYyphxe0GHvhJT7XtYT2YOf4cF8B%2BgpJUBe6V4RXeuGn7nTsHRkwDGP9C399TgWoSGjU%2Fg6C2Nf4qte3N&X-Amz-Signature=1fd5280a698ecda0519a98a7ab25df4ec70b3a1bd58b834b14e035c6a90e3ded&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

