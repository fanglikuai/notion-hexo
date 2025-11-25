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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RZ3MFY27%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T130055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHXHNYfnlEUCoTYve9O2fRmLyZzJFOq8JydxrtA1vvYpAiA3QLf8AC9SiMrNmg%2FRQmRad7HSZb5eNq1K4xfMvpKj%2Fyr%2FAwhtEAAaDDYzNzQyMzE4MzgwNSIMyOQzT61Z8Hz7XHpHKtwDqru6ksWW6%2BHB8TK0Z8TIPpu%2F9BSh%2BNteD9nYpvxQP%2BBrBrXpBSsMY1qWXl3IYzskTVc2U829tJ%2FDkmLZ7pp4DXxxNwBrjHXuKXQ%2F%2Bv9%2BGBEp4WSeYOBZhgR63y2Yt8NPBtEf0waDPBcyEH1PMm4Kb4%2BTk%2FU%2FjvNRNmn6LZy5M2LGlYDGuvHojq3VcIwjMbrq%2Bk5B9BRRDn%2FWuvcb0wenNlUjPoZS30qYlCXRUEkU8ZIR2nWHBJGXNjSbqhvZ9fKKHTuUc6F5ejKUnL76c6I0Rgqi%2Bv3EB8E11UfYquMhuCiZaARn9YzXwAAJdLKKZEEV2wd%2Fy5iRvTRZ0J6N5HXbTfonzkIwlLFb9Bp8ngkpomnYvegz9BsxxgNa%2Btx2YLTxH7jp4EgHXqMLbPGm1nUkx9dEicQ7PUk6G%2FRr9mW3A4wU7V5c5UZvW7AFF%2FRknwXB0qDLKXBEoErxL8NpglEFNfhRBnRReHenagVMQthjY7Wvo7589Vt4y8gaPMu%2FlkFJx2d0doRu5ynSiApjXnmiOit4roSNdeD%2Ft%2FoRheuwTkTGEK32di4ri0zXeN%2BTCt9kDMmjKvl%2F%2F2kn2K7Vrx7LO1UBXsdkNtFAPRg828WQfi5SyUWx2K5ohtPM%2FFcwwLWWyQY6pgFt1JeB8KDLwEWOvcrkJvtfIfIjB4JJkzDnooS%2BF8%2BMNZWe7QpMjO696HYt3960VLfgj5RJXbwYlWPls867osAjbykO0uhf8OKonMYcMqbuCmpqJAUFM4s0pasq4lIMmyyfrGhF57lzXmpg0IZhvSZQ0sHSNKegRPNuskAnL5iHQZyB%2BOJIePitAJmL00babwfCq1GuXyjICv4zD2iZTdJNJjpT%2F%2Bqw&X-Amz-Signature=b4c36300613410318f4b164367fa07511d944d810a6fd9e1043d7604d6630e5f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

