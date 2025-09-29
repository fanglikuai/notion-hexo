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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UDOPS4TS%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T050041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJHMEUCIDxIa%2F%2FIrZp2xRHTZK3F8gEjQzGhhornFLvQJRQPkj0bAiEAljx29AEEjCcUmw5L3MxiofZKUUx2VfWapPi2GE03eiQqiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIilQjwNsMfdNcx1NircA2YI0tIOhPrx7CDRGDKuzquzhnW0Ahvcxbnh3CU9aVLAr%2Btq5qTliLGe9gJpYfH67ai%2F27YC6V2IiW3UmvYn2pCvexNQYHrdPy2bApO2qZvabHQq6sdWvZsn1dKX7Pb4LLcK9tFZcIv2w42SHucLfGOPRlTyujtC0FB4XrmVyTwV5ImXHUf5KE068I5k%2BsbZGNUBgO0vJi2HVirs4Q8%2BMsThGEHMKxCclb2IRmZqlAl6Z3O5GbLXodmaTL%2BdMHKhhV6b%2FhqaR2alTHE%2BDgp6yx6zD3J%2FzVxRVx2rdmftLDsNG5lc5DKuN8PWeCsid0iFfaBwh6ihAjtN3h5W0BxDaVzYbeaWkLjhOJEavS9d%2FuiBKt9F1EY9FMBPEjJtXofIDUk%2BYtKBM%2Ft95byKs273u76GDsisZayZM5sJnse8bnvXboKqSY2Ov8KS0gKuhyAOOJZXkL2AFInpObDhTlYHXalwD5IJNWr1DADVtLXA9hkyvEh4mIhdS5dSYy8w4FyRu2%2BCGKaBtej086z%2BHwEjPPwL5ljAHW3qsfHE2JV6Q67d%2FxaQrx7KDbJquiQi%2F69DNp4c2LU4Ddg6rcYpGjFngOy%2B9hf3KrCho%2BrtXfmFjbUWmfeMK2fZdr%2FKRZI7MNOq58YGOqUBJV%2FsbFP42cJf9sSd6ZgwZD4xO3Z6bavg%2BccrzGU6qdtKrCZDzqLmurpDbRvbGP33pOkfGOCs9hVbDSxXBGdiBcDkU1rGcwoBvREuA1zNYPKaXKBWN95IOZ2qQzv4fsbVxvfaBp0XGbT64Pk%2BbjsqI%2B%2Fe0o%2FVgdOveR3YrrdZYy4dRNZa3vkAaPaBgfUWaicoyV4SExGDWXK8PJfv3NjCAEozrz1r&X-Amz-Signature=7e7a14d64d4fa9da58f3fe78d4656389e72f7cfd6aaa5885cf83ad0671fb47bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

