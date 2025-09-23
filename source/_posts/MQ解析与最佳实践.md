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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667A7UF2AN%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T050039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGCpRW79XeYReTLYK3qjdVkrnyA%2BN6wo0wllQNUf31cKAiEA9Cu%2BFegQYWjeNza928TLcNQBhK1sJE7svBmA8cTyUvkq%2FwMIPhAAGgw2Mzc0MjMxODM4MDUiDHgKi9%2FZoxAmSuKDKCrcA0Dg06ngo%2FgBPv%2BktKmzCBCUA0MvT%2FRFfhE79JCatjKRt5RxOJJ%2BMsfa7sET3JjAe2Fykc3wIzW7bHlpjyKBIw9mgZjUUZ9r7zcfiN5%2BCyOdEFMzw3TiTBljRC2YFQxf2AI34fPei4bWy2wHI%2BZ%2F03A9ZS3Ud4kDxmtN9i9PIJ6IGCIRvgsYvZ73Syf2iILOdp9vE7rxeZOWNUB8QfLasQzUOjMWT3jR96G1EPXdgXIyeA8UGk2AMLDNhfZqB4o06XUARrRnaheS0D37K6XugWxzkRw%2FFbIETiAvtaA3RnTWkkjsqapKpkkiGizniuGDCvYeeRZJBC%2B%2FAfOtwGRYnJGL1Pljv91NzUXc4HVFtVNdZxadmfMDDIdnzJKz4xbtNj8cI8nx0iw6%2FhQTfxQfGmEI0xSBxHUC4%2BXYBrC0G%2FmLn5fMzpEWrR592vY7Xfq5s%2FK5C8q9D48jMUC812%2FR5VMp8lNMA%2F1kS2kd9PbomuGlOoMjlzmKzAc9LcF%2F4cEYit2m9l04YjKVb8a3FRsqVbV4vve1BsGC42sTrpLpXGpjxhTiIkZ6VBsOPsJTYAeYiyemE8xII%2Fg9V7%2BmCS66MCidYiFV0bEA4bw64RtLZl2dG2xmczHUOXBLEhk2MJvQyMYGOqUBctcyyzvnNJf79C1gaLShOM1Ls2WlrfXdpuuceKIpScxtRFZ1iW45Wj2jlE69uieFemKhQqtAEspDFY5cCo%2F76rtnRSyCR60vqc%2F1KMy9GQ1bCJX8p%2Fqlgi1rs%2F3HCQO6QRhOUAU%2FpfMtSF9WlzSQeOTeNIcbtUSXp502ugnYgWKhW5V8GV52o5Sa3B%2BBy35nfh3GfOs2d%2F%2FNWO3xzwi%2FPMHjy6mG&X-Amz-Signature=d2b950f0d1fa462dfaba7d14e4828ef2f5c51d9e7ed517c51719dd3211226d9c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

