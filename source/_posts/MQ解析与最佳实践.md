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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UR3NWFRL%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDTpmFLCzB71TFraJH%2Fe8Sh%2BpY9FmeH71Tgqr5LPc1yHAiEA%2BAQI%2FgTCCPocPBVuAbDWqCBT5sGOoHsMlU3Y0I82JT4q%2FwMIZxAAGgw2Mzc0MjMxODM4MDUiDIvb62czUzuZNQ6WVyrcA30OUbv%2BzEW5gIw%2BITikcLUYLAOaj3CUFILcItALLYIdtzH4B1GiMBJkKvUIxsDj8iU73ncjwpQbG9b4xA%2BLhnc9r0FLDGaGD86i9Xs6o4T9CjSkFvraLVAWHxSCHwL9WObEr4JEP3rRz7o0%2BXOoL7zLxC7GH5c6VrL9P%2FQVtumvO1kOrT0OcgcSy0CJwBqyl%2B9mgMetGdGw5iBH3m3ZO16QLLVbFRVvVWJyUbdq57WvrRCUitXGLaJiIC97lzLlCSZqHzGly3mtw1frlfOVbyRRWT5irFqpdVA3gZtuOtFlfgRbeiwb1I1BzF7dKvICT7i17c7hxHb9H5QbjJu90VrVgkb9dpt6S%2Fw0unReZSabMB0DVVxCizyRFbNLPIw9OXjetk99eVj2Krp9Not%2FVyCUn4pGXHu1ZyuuW5mmzvM64eqcTJTTp%2F6aZlmBoFF3HtDkmS%2BvokOf4Tzs43NqK0AJ4d5UMoQflJmIkuqjE9j%2BVge7fmsWnLlRgoHw%2Ffnuj%2BWb6iB0hBcVtkCwdeqkBavxqr8uMkMrPs5g2MriM5kyAYqiSq0lXmDODftnPbhT9hjnpwxTXhEEsvwlwB63gBi5qf0CVGpZl2raOzjRTHq4RGvk9cNDlnCiIDlaMOqDu8cGOqUB6BbkekKcr0Juyuf23Vvs3h13BlVjf%2FzPzI6tmwMVN%2F9ivVpg8VSwWUfNVwh%2Ba1q02n1tK9SoMqpVuovVUR63OhDILHf2SBroo8Wl7SD0RBuATzpgGz1UwicQlkMXaveDpXlkqvRuo5%2FF7t4%2F2J%2FtS0uqUjYPVPp0LszfvtJ91fJqmhqy%2FX8qjuBwLLu%2BtbQu6GYOrQafD9Ug9zzqzqCRnRlbdR27&X-Amz-Signature=a30c67341789b4d70a817a2af051268277f233edfa6b72620beb2ecbef46f744&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

