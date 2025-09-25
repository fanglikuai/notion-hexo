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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RGAFW2WY%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T170039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCk8K9cGydvFFRMNyhxLmFwptgNLx1bCYbYEguPlzpJUwIgATMDYKOnagcYmTTIIMcU2zMzrwOGizQ30zrlJ4Hnz98q%2FwMIeBAAGgw2Mzc0MjMxODM4MDUiDN0DNlghVrF10AxQSircAw%2BLW3I5Je65jLkJC6%2FIJUugdjU4OeyYt2cMpLW2Oag7%2F1m1XPAfptWW8Zqi985KymgDrexOzRBYwFmBcJZeq%2BsWWabp4g%2F%2Fd7Xo29ZxW5OGxxQ2qPZ5FShDCuA8aM5pWRn5fVK13hQzDefBTZyLG8W0lVY793m2LtexWKs2f%2FSk4SIcQzsxwhLhScy4kfb5Y4iiBnhcEnBBLsqUaffaWTGHdQBbxVwtDR4X%2B89T7mRtzVU8egUaRDEfWIcyhi%2BnIzxmLawDD0sKS3s8mJa3yBlf4DpnCWj%2F7sTPkaXo7ehkFzrLMO%2FcoR8YODdol2bMHf4gPllnr%2BA6yYMpjVt8NiuvAtex8Kjaw5cKCjo2QCiV3Kw5eojAKXqoHOFZuaT7CAlC6slXEb065FO6ugsyQbB0LkH9%2BBIWgPukYiQktxVfnLdAVb90wBGRGgZPzPz6y4RDV%2F4PQlfGBxascZEgBbdtFnwE222uxxIWlfndBTiixjL8Gww3SZI88MO0JW0jfzv4WSnt04kcUU2kOWw49rGKDtsIM1YeyUOC1rfFuMS08QQGyAmshE%2BGKGDMde0a2U4eEbBKOR8LRKuMjwZNmhnKN89ZiRo13OSBP7QQ1tAiZQKroHgdSRvdUegwMPe71cYGOqUBRm7Y5QiFGvhrd1oJA13Kb1yDNmIHt1QiN0Nu9YYiTMZBzovbUi5Nn9e%2F2Mf5qLSIFj%2FKAxlBh21cWVFrlrmS5kJF86Vp2AT22IJX90Bm%2B47FQ196je01RPV281t2ejsYSigTiC9fPl4aTSvuwGeQ5uk8687gMZ5Z7zi9fyzor7SGKgL4wZ3uukoiBx%2F7o%2BnuGwdLPVgzFezd0a1nP1XOLEiN2vPw&X-Amz-Signature=6a45023d61baf2bbb47d71694227361c91577cf51fb4b960db300074d75a747d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

