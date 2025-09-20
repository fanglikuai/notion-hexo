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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662EFWF4PY%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T000040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIGzSS58ujsxeORYUTswkEctP8ulAI18k%2BbGoeXwjZzvqAiEA87OEnDCGmrljauGRNUe9a%2F7ihBvUh%2B9ILBZSXvypHTgqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIxbel9EYyBmPCQhCyrcA1lDtPlRtcg5MDfd7zHuczUNGMifcG3Wt27TVOMg0tseNmaPKLmfAs74%2BepB%2F4EJb0R66es1hdCxm%2B6Lg4bAr3YKxs%2B9zPrlMY3v2m8fjUB2TpraTWq%2Fx5PGqlDOYSkRr94Tf00pBw4yadT%2BfAvutp4XmSGYPJ%2Bwd0OeQEZyTAtZf7Hf42e0BoNeKPSdXalrT3uJqcdH%2FM13ztGTbdxibKPwc8S6fMdTHEhq2o4FN8yJIUxLWMMlq3tdRxi7CP0y%2F0ggPphcjhqMQBotPglE31CeaiAE8YSbu7AAPqeXdhbypyVhQDojuaCMg9nyd%2BKm4lbdDrwjOTJSePMowRkT41Ke%2Fx3yaXQMgr%2FWKSf7zeHzkiPJfIWYcQHX08anEBaqMnRb5pGkqyFrEJtD9K9k1X1aU9WyeEtjYfoprnRWy1Z7uOzYdk6KUA4Ev7NodvBVWd4wAT1%2B9JgUw20snLOk8LEp8fz8tJ7OOnq3BZqwtHz2mxuB%2B7ygUcvD9crWPl0eu1rROJmPGusbkZ0N9dj5HkeS%2BR5hW8%2Bnahr%2FL2BfOKd0yAPakvRYwB5I25Kh3aIhAeFXyIVoIKCYmr%2BX0synEATNfBw3cNkISwmSpYXYbYf0B0ZATSTDoxp01Q%2BDMJ%2FOt8YGOqUB36Jy27qu0xJt5YALq%2BmJZLIhMqP%2FdhuZF362DaCeC%2Ff5X3EXj8%2B8yw54OPsg4S%2BeAuTCbsOb355N1QY0dWLX5DTZh8zMGIfTri7xGrwbws%2B%2BaF%2FNVHWxkvfjFFkSL1RRgxlHiy%2B0sbRwTEgbLbRgXOOGlppe7LwtAcsOSQ%2BCwXzGZO19Gp493PWiKQf1TeMOfLb%2FpxH1AaK8nzPdem2UZfJqtrs3&X-Amz-Signature=99d2bd4627e77e42fc1dc7051b1f4a32602d7c93ca39fcc1561eb41c138e1a70&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

