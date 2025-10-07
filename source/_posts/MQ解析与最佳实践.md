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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPGDM3AM%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJGMEQCIDyPoZOcni%2FjoCtyoSi6ijPCJcWKmb9cFptYJg3AOcZ6AiB%2FC6HV1ANl64Z7UZ%2FaEfpL1CKzoC%2FYg7rIAl4GBocMeSqIBAia%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMcDFdZNDUIF3bveb3KtwDi%2BwKzsgQSKCTb6pSbynevqJ1H%2Bk7P5irY5baQTddaxGIvF05lmkWKrKBUm34%2FFwR5789huHZ04SkyHdUofZUUnWf0H64BvTURYbLoA1i1KBSjhGmB0YxPbHY23AoJsZ8PbjwUhClZMrHb68GRzgAU7hQMpfD5rTU0g6gPH%2FXEQrdMdcau8Az%2B%2FpsHHRbuuAjNYvo7RXzAk%2FlTFlVwmZijoMTfg0r3jIh5wO%2FOnIcWFOVl84K8LzfprwKfaG%2BVdnN6PU25ZTYjBl6vgvFNOGB39XtMYtgKX7dI5Rq2ifsplM8UbAT7m1iO3czcSzaV0U95EppvwLR7OCYGiXOSmJDcrUnxN07xsrr1BsESFiPNYsmgSpFq13PzwCCPKQaOD2VLdBxcp8YJiU2ke38Uk7LOcmP%2FWK7fn3Vg8zBMdJ33gbidn1kgQhfun6zNOaNXDbZYwdRbsYOv2P4LGmYk5FIBzn0hPDbR%2BaikGgUonbEGOKlCm9N2J5SFZZOnLGTGQSMO2DM70wQKosFB6ZCQOQAoSCLuBP2n9%2FdMZeH6JHhuY913icZjpkumxrlwSlaZOM5oC1Ay%2BYLAbcrxQnKddlnIzXw7ZxzwBXLM1a5eZrLoLFCixNiiaY8S2eYB5oww9WRxwY6pgHftiEz2tUOuAD97rrDxuz5UPOyUF3W83f9GeAgVmaFm3wqQtGt73ielpPH%2F5VXEhUb1psC7RatAiSyyMHka4eVjDReko4NnIAgs14znDrXlYcvGwWbYgqMWXIMHEHwlJnwum310mkH1KcR5j2Jdo%2FDZedca3S%2FoKB81jevS%2FhCazUZt7uUv6I%2FH%2Bq6kDVr7nOnSA0dMu8sQ%2F0LauvMT%2B85l6r5J33c&X-Amz-Signature=8f5e8bf5a724113f8b67b538e19276e90a08e7b62fb9bf6fffbdbc551c028026&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

