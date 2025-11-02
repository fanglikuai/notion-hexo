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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666E7XIEOA%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T150047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCICTVI2dMLU4f%2BYJtweOchyGEQKXIwy%2FiZ8VrbwLV5qc%2BAiEA6d7nNJ4ZZ562XViN8nc4nCUtR2PPp%2B%2Fr%2FH1izc3K014q%2FwMISBAAGgw2Mzc0MjMxODM4MDUiDAO4IpNljg6%2BwEONHSrcA%2FWskkJ%2F1bFPdHNGfMu0RGNOyeRGcN9GxUHudg4uUNDjQrlDp%2FPqR5jFvpdDfyVAeB1xmS4QfTf3ekb7bsgMvyAo5G5yYN7TGmQqw9rj%2FDmo0rTv8xmoG2b53%2F5MyhGr7cUfVuGnpL3i62k2F9puGgIGYvYkLlesKu%2FkZthH2f00Sus8%2BHhd6zViTibjQjx%2FFH%2B8iIMgWOQgLhFgPsAcbliPI2GUuyi0zxE1uMMHII6bwCTjM9IdmHw0PCpUGIiGUXYnAiqLTpq5dAJJUCidhObKv6cqTLeQv9H2Mn00amkmWhvTbKGkK8ENWPRBBmS8yikTIQx96lEp4u1vL7infm8NOJLH4HNdBKbg4k6I0RecT5llVFg%2BE2e2Qz8c%2Fx86GQgMLEJcSi0Ev%2FZZp2CvkWeNqLile6yZKuYgEV%2BtJRwt3Hexip7XTPIYoR8A4M%2B8QhPlX8QAgkWAuV1Uw%2FudTeTvzMl%2BEM8NlWDeqwSt0X9%2BnCcuXP7QP%2BYDTUVGoAwuPnAI%2BkVR0JsITpGD8a7NNgu7XW97xSO%2FF6z0sPeHtt3KtCknnltJT%2FAWLWRLsrrzBCjPnxYEi58DyQ3DKMFTy5AvG4IV7h1aJurN5Phvgki8of2dL2T82fl5ejqpMPPincgGOqUB16GHPCzfz1Do212cZuldjICxhT5%2FqJ1aVq90cR%2BPRdNrwrFRDioFfY2Y%2FSfebqApjXckVsYHfkBBwY7AjIWMfqJ2I%2BUb37WT0ko2UbZlDWVl40KbzjKWQPhKyjfYiypb2Oh9aGmLzVZcfkJw%2B7cMkFhCuiq38%2FohoPdtKAOZBqFXCrbDNVbiCTV9JdX%2FP82QSxImdMWbGtSs4fO%2F8YoQMN2Gmxut&X-Amz-Signature=c8cfe33d9320e3c638038d5682d3e2399b0432e41f69c361f638abdc459e1916&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

