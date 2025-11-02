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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RSATOVMT%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDxCbxXONySD9sDGpAiD6c5wU9xn2gKZ3Ys5AMWSukFqAIhAI5CUGOKlWvFixH0CKX3MYv%2FboYMycBMg9IIDTmtA%2BCIKv8DCEwQABoMNjM3NDIzMTgzODA1IgwgkrCzhi%2F4bVV57cMq3AOCzHM%2FXCrfHtH3rGPyRcC0AchHkfOqcnq4P49H8R7XYFJIpMfEb%2Ferrp8CA6tz4GooKCc6dxMwYMuihR2N%2FAbl9HT9DRihy0%2BNwdvjDUrHrbPryR1w9IK16nhBPBnyt3%2BWCBZi2BNGptN5ctyfgixTAk6GVBzMkQZNg7snmbj4OiIrSetUzQRmqt3ssrL36mt%2Bt1dRLQ38EV65sW%2FqdXGd7wDuOoq0%2Baxg4lltEPou6AxHixi%2FAxr6qgILZEwGQ8bG%2FdMnv5G0QgXoHHbqBBc%2FKln%2FJonRv2LEZ9hPEFbqOgapXUtdDz31JDOf0Xjpm0jv9wbBpR2MMQDG%2FDEEizfj4nnpqxFcQGFith0732SNhl2m8tocwEhUBbdFu11TVbvDT8qBrxcVuXz1qRxnRQAJSrQO4qeS1RNZ0ze5C5x3rbPqE83iWOjXmB1Qq1%2BNc8XzOM%2FJIM%2Bm69Fj4RNY3XFkyNaRNsmdjSO6pI3p1hoyNoK3eciPOR7qA7CBVOHtSHSBJSUi6gOsezOx%2BYVakqlKNVH%2FzYD0adG6nrU2C6LZxlXvZwEqJU3hIIQokoN%2FSq0JCruRH9IFAiBFwMi7QeJ6A%2B%2BvnHiTgRgvP0IpwEJ8IZBl8zrxRZFyftxIjjCl3Z7IBjqkAVF5SPTotNK8sxJzMz235mpnS5YNv45Jzv9WN1dPf32AfYVJBeSU2kb%2Fq%2Ff8GY8KlHT9fYdRD7G2C47bCowtz%2BVbja0BartMrjBr%2F1G65vxY2gfYxqzp4XqDOup9p2PmGjbzXTOC8UxbtuW8bmPQDRISpRluixR74QSMsiwcg9w1lhtm9I%2FpDYNKKcBxpuR4yLMqnrblkDgWxh2T%2BIK1zAoMUqnh&X-Amz-Signature=3377f9e0652ff010e1ca2f765d4358218125d23df9c1ecdc5e77164dcf6dc0e0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

