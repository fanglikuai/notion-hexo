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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UQO4QHSK%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T190049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJHMEUCIFnQb8oLnxhlxvkPXog7umtzy72MSTB0GWuBFcwXF36SAiEA5weTpRpvwDVl9PlU091%2FCLGjYvTD85ZrzS0jdhIk6XkqiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIuQR78HjJd5FUcZbircA2EGCoPC9Z1%2BZ9JOojymoVzA%2Bmg6sjTJPR71w1miBUjACiTv3KYMaaN%2FpQM5HXeFLC8WSqui7Yt2P1VHHfBSBjONGboAmT5aWgOcpAw8oqg21GSutEKihcCzLdy0G3QQZD8fpsPQecPT2hZrizpkN0MsiGLrq22MuhJt0ifYDQGZCMikaBoz%2FR4tHye6JpzzMhvRKZCd34ASmWEbeKWImkPpQWsANkDPMeEXy%2Bll2%2Fqod8NhQKcXdJ3USSr2ZdcAo%2FpjvYnNDUJJZkEMtgYq2%2BSIV2XAOTnQUDgYreeY5riJqbvvq%2B%2Fa%2BKrKzYadZrkcR%2FNytRjt5wSTEfenbMRi7jEYToWS4ngRDif8gfRdsKt9hgBP0cwaJQuyXbbRB2gcS5vj%2BAsiiiCp8Ko6FKwwqldhNxR7DzuIlMEv24w2bPnCGgVjkT%2B%2BWyJ7%2FLaNJkT7Fn7n0ce8cK%2BA5%2Bc9NFgD%2FRFBTm5gfCJv2bp6cWPregP0OvbVyaohbAr80ZsgELwEHEKBP%2BzwVSjIX17vtEUWH%2BTcxJKi6V3Jy%2FFL8YSKTAnM%2FFXH3xx8PbUT1d2Rjx1ABSsmtyQdaOIsAycgTJw10HKI%2B8sMbAiJtPdf4JDkTgqEwivVd67vi4tuSSeUMP3Xq8YGOqUB26x1%2FUU4SPP%2BjHqmmbYyUjlFa%2BKb5tkT1VOnoJDBoL4zRDS1gNwGYgzCJKbF1IRvwH6mxo02usNplr9xEs1zdhK7SRxSVeg4imaGLNMOmDvrkThpSCNV6MTBryb36MmH851bA1fmjU%2BWw3rDEIqEPw0bL8iQ1oFiuoCeVhfNoOJoIwZPxn0dYycj43fSP8mMS6m99bfvbqWv8sllM%2FB9zLIdHHjG&X-Amz-Signature=10905c5e2b20d00688a73419a627824f429a20a3739491642abe9f660365561f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

