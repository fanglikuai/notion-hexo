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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SCH3E7JM%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T120043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICM7lQX%2BMFSmCcH3rG9kZZnpc3dd8jKD3U9ZVBBpJv%2B%2BAiEA0Sh9jOo0fjGrPW%2FIkF3mFZFwFvCAeUFLrbR779dWIpsqiAQIhf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEKWCv%2F1GG0OTszzYircA%2Byy1rBjqlH6meOkv6HOECpGbVRygBUWU%2BUpIfVn9WjQioMVRo2r0YabYfmO7ByUgKKEj7nH5q01u8bAIZzbu5wG1hJCzVGBzPPQI6bvnv4gfRAp2RjlQWZ55t%2FkrDsLB4Fw%2Bp5CMrUx98zOZ5MvYcnvpxCCT9k%2BUvisTVsH%2F5k0o7PNe0JicRRG%2FKOVWV%2F%2FRAAY4ldy6m88OqPN2HI85GtBBpziM1ahdYa5cJlpfC4om2ZmHNwHkWYFupk0W3ziQRye16GhAOkGUP43xuiUWuctyJt4phckYNFa3zCo7oFDt2JMQ88cK%2Bh1GJcO62zEgDYigDwOhicGxDDmcbdPSV7DB3YJ0K%2FmRNXWYbJ1lQ1qqTr4UX%2BdsJ%2F8mYgMKx5gjq25u4ABJZYij74RfMYy14a1q5rbSvHjKlcWEaUvRd5cJRL%2F2iwLEbayG2ZxMx5%2Fi1BeLyMpG8wminlHvblSLt19WV%2FDqTNBAr%2Bthr%2FN4i94rqpToL3hbaKi3EP1lYP0gx9O2g5n%2Bil65mIEAK0HCx9GxBKNQ1zNHy%2F0uoSJHsPNsO7oty4m2WyHUIKtGF%2ByJuol7BethW9ZoRai5oOzYSDihpxPnuc7WjWXF5X2NWp9vf58%2B5cKIwbxtuo8MJXKm8kGOqUBQP82l4M8URZ83ze3Hk5%2BBUDRINGv1muymzq6tDZ%2F6fCehtdWWxscAwuppTyGUBU4WWjdiz7mOrIkElYXeBqQmHKjNTc2fCW7nCkUeNZEUxLhB3c324EfpVwXZYiV%2Bm8IO6JLBycJUl%2Bu8rFtUV0J4aBTwMTS4yBnDsUvOIL0ZIGIgl4UFA3YNqujKm%2BjDcbAAO5ANppR3nwmFZFKYR3DAjcPYI81&X-Amz-Signature=4daf2edc1af69ae30ec079bb1c8f19deb095272f93e8e69dafe002f256c3c542&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

