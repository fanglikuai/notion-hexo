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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666EZB64GZ%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T130040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCjR0VIzv1QZ3Yf37Gxj%2F2uLpKe3bv%2BBbDIm%2FvMpDHSpgIgYTcezxOPiacycFiUCW5kVIv95WWLg9bKDkBTELGTmwcq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDMIqJhaw%2Bam3LlUPdyrcAyfyF%2BtYfDRzRQtpPPF3Zof0ppghHEDyP%2BzROqrGdBQhbyfgMxKhOFSGCahIny5JpU5qqIJ7QKlSLuBjWmR16cQOuz49nzDkUXxIww9b0QggeimcEEpVCtnaEzO11VBv6QyYm%2B4dkZ39Ep7%2BY2nBJYrKgwCy1IPkVyCFIRZJyuvvBHciNyVuyMMTJQu18WhTqef2ug6XrnI3uQADm1kkoVozq%2Fxx2a205%2Ff2ohpNoKyaXV47mp4fLwerLJodBw%2BuVIUqqPeBnuhUzRyxjZxnOJUEUUMjj%2FtC8xnwsf%2FOpsYrZRbJ1H1%2FAyzjeP6QSSn9hsAOapLwRgArWYwoJ7Pk33V19qX0FfiaWtILjZYQ6xbqyOpnAaZNC%2BLXgXHGfl%2B9xWQYiWZqJQn8VP9cyZ7y%2F%2BeVtSJWXhQJA4EHxuSvloHBUWKk0snGDrMjZzzgF95QUzlpjfv0%2FSPUa9aspJbWfdN0Vmn8KlXRq7xjUYZ1M0OCEizz2cY71TynXMSvaYzMCv9fum%2B%2BhwGraV5mwlPRjFJHojAreQEicc80sN4RPmT87otB%2Fku5ac0bRt8LbzORiA5qljYFKo4d2X0iEus%2BiPr7U%2FvAquayECoQwnC9ZFh2n7dL7wAa%2Ffpv1YDeMMiiv8YGOqUBu1UouOHNYDXFy2HJA2NzTvvPJglhgqx6OAqR6q17IjMf5wmd8Y1GouE1eN0fcL48GBJkp1NDwNkbX21jmVbEKak4f3JiP2lwYVyowJcozHcxkQdZdPSrL5HRCFOBrwplWebFMRjVf7mXO4lgjIQllAaw7fRvf3g0kiqQE6QYoIdp%2Bjav6Obp46gZMtWhqaRcgRLLuWYihIc2pGJXN2t25UOI2dSo&X-Amz-Signature=8d573b16020684136839beb87ae595a1997736e0059cb43529d3fe942992134e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

