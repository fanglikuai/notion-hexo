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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663VRDHPBS%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T060051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIA5s%2BKR9babMCwiBo5%2B3Unuja%2BmmejuxgqQpPotNiaLOAiBuzhYRe8lYrfkRtWofMX2cFgzAZQLtCYb8mwQKNzewrir%2FAwhWEAAaDDYzNzQyMzE4MzgwNSIMQ8jVhospGpE7UbIkKtwDEuVUn7zL3djXC2owQ97xNwZeGF2mVPZr4XCMKr00Pl124hpovg9s5v7Jr81yeFfjH%2BNUmGEhjjWuLBWEOBU3iipB%2BIbG9uIPT6jQc9bgKbmWunCM0ovNukpdn8S63H6dMDaOgc6%2Fypop9pipQ%2FYUJTriYoBuWNlSQZJf5Jtd0yx35IHJWaAuwtf4%2BFKFCoMJOSCxFJ1VuGHV45h38axRRGn9SfQ2V7bagI%2BwZ87FtkTUDcUBP4HnBBy6fVAvJHyFKN4xTN5V0ISKpXX0olmTVif%2BOG3OPz4ixJsv%2FuEa3Esgi4agxbUzz4R47%2BRtBwYkc7xzLG6sItuXTLRwVM0FAna54dXm5yHqSdLJclagJcNRb%2FRFw%2FUceeJI7tYNklXpGmN42dAh%2F6OzHD7NoEWz7t%2FYKBgVEIAbkMGFP7BX8s2aKaQW5yczrix6SnZ0WZLJitq%2B2KC%2F8ban6r6hc3hsODn6xlXMSmkcfAMQ%2FsjHWNed5Fn7L0uORU3ztR20nGiPQijc2rB3kVReVlpDGcIioauLVXhJji5muQlYX5FNqUGIqJZvk63kmEXjPQSK87ZM7vt7LaS3FrcmRBVBSYTILc0Gh7S%2BlqHYLbfTVXcR2PhhRNVnXyfuzAKSxqYwtrq3xwY6pgHI89AhZNQxn8ZXQ4M60hjuI1LhmjJ7nmSwbOUf5whoKHtojQ6bLrRyJ%2BQj9IyIDnux2qsdHUwBAS%2BJgC%2BIuyzMw%2FgzeqJGp9r3Rilr88V3OS2smDQcv1rBp5t3C8vKXS9gv1c8Dw6mvabM3jYLZGeI6vF%2BcNs3QV%2FWu6QYCmpjGPB8A7vp3qZoEgtgQD00nSeoAhYQO01iWK7elUaKDQk8LDvst19s&X-Amz-Signature=83c4a8f3f2b43a61a7c498072646bbf4035bcd7a0f1c069ed832bb6cfdb9b049&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

