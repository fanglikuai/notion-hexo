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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UWFPDVGB%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T070040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJHMEUCID7BJ%2BsQtV56q6tJ%2BepmU3gMN1iYUBHSc9tG3V5Hakf2AiEArwawzpKB0U272Ex9ItHiW9H9VgOdWWY3BYRvPuYUsu4qiAQI5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEHUlagYYm08YrOGryrcA7YKbVvu74DLkrFxwboKQ9LLcbCNNACmnwqbmV2WUy4g0ybp%2FMyYdE6%2BtWiPRI%2FiNqgSyRzgUgPC6YSnkII9ezR3vVBNe0Pze4%2BEdeVzNQzAc3pBeh7WHgBMDxveqCFT8SrRIZBwUGdF0LPQx99SZK537OBSEZ48FNvPVEkDhCFlHoEItSw1QCxl8zao20uFOE%2BHdZvBAspTyxE%2FSR963N%2BnXchNQ7%2BKjUZbUYL%2BYE4UFnU7YSjKfJchpTD4y8%2BeMpf4DuHF%2FR96QDzuvPuyM%2Bz%2BkfHYvBH5FHMzQk%2FJf4pkYEw2ly8gNtt481ZNgmzCiQjq7Bh6gOzSPFNKqIyC53hAmItQvWsq6BlG7C3p1LLCbDK2ghgvajGtqt%2FbdKeML8WLwVfeYXLkzzir5EjH2psfFWXKcWY5LiXQu%2FpoRhd%2BGA331ufzpelGaGpimwKZbd6RZXJgCN2B7r8wYdRrBiLeTLZXxXdfeWsO3g%2Fz7sZWLWN5YnQNDtfJ8bHUuGfbMvO42NJnljpE9LR8GMrOrrcTCzC%2FjPFduYxkjOvFjVN6YoZ9wbZ3Lm5BLpID8ycOW4c8lECFOnel8KRHKv4FsEMMSTSD5xfbIHwBrtrDvpWDxV%2Bck3clI7qzcUK7MIHm7cYGOqUBxKdKcKF%2BiJC8%2Bz9TGEvURCB7Bs2UUS73KHSeRaEaDU4CKGRTEUSM%2BZROfESSHSjUZFwJbP1so4SV2AHPY9sD2RfoXs6CCiYjqITP%2Fo61tJgBhKyiEafGyT5qNVGYdFnL5zHegiPIJAArUkSmN%2B6OGYFiCfkEGVGGmvnlLvEYRqwBIIhBJ%2BgLgvRo%2B5MZQkZXy5IoTb0aLsTWbJkN3hC0Q42FpVzC&X-Amz-Signature=cf8ea0162f82b5b39eb523eff4949e3f24066e1c94d8c1def2466b671a06e290&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

