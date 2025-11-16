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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TFF7J42H%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T120047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHht%2Bb1aHsnqFKS6s6KdezAinsFJJ6h0H8o%2B%2BcNLCae5AiEAiIuqsX5UNSMIQtuHXpzOfZh3CLpHjuyMENMZnPeQcJ8qiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDJoFZf%2FbrVBJcmNISrcA7BDoahCiwB3Y03pLR1lJCms36z74JnyWpzNcxPWc%2FuMYNzY%2BoAQw5MxY98KWRvKyWyKFpyz%2Fg4jkyncTCZUKwI0J79Qjs%2Fmg2LbsshgILNxbb14npYfhh5UzeJrhQO6KXI7v4vBuKTf2VGQ1Rp%2Fo2jlyo7eHYGUmypldnGOfezfDhq%2BJ5ioP%2Fomd%2BtYmeazPNG54GnC2i2SWV73N6HT9XrIBJUCip4xiiL0sHKBE03MeaT2mYos8NOHfeACOMnt89ckWR%2FgLXNKW5EWZ%2FDUwAg3OUNmKORYU5xQtJmX4Ro98uerTR1PvXdATjxB%2FZrKlHweeCAEE64q9VGB0YXBRG2BKJWiWP9ZWU6HavfIaxNQgCcsmfOlMrSDd1j%2B%2BNozjDh%2BiX3TZOcbRqVKM%2F57jBnFqhEWtVngIzReuvsb0Vjbhu8bmkGn3u7GzsQxugL%2FUUFJO2%2BdRjOCqU1lTyIwVSgu74lOCk5dWtqtxF5%2BcbVbE2VTmgxnLIkCRSJQSbSI0QieCHqPCThK0TOLJ1%2BwbJrXTYt3LEF%2FmbKaMHF%2BCpIgcO4lLB493i2C7iwuzWrTDWRh9EDLVe4V7aP05tALt1INxgXcJGZlpBzpWZAGl2dhB3Jppivy2vrEIUQ8MMT95cgGOqUBs8cqe6C%2FS4I4MKXcSvEbyWRAmR32nwo%2BA1KtJeme7XwMAlPThHSYgfOoHJFJY5%2BgYoiF%2BhO35%2FA9An2J2OJO8Qjrj0gZXo36Yw%2ByYrv7UU2Kk4sRmMcI%2FVeeE1c4olr18k6yy78gjYyGekHjXKEyH71UhjKj263Il6Hnva3RIW7KYkgXsQFKe0iQNO5oEmFz1favomFVypNp%2BbWd8BOI3AAL63ua&X-Amz-Signature=9c9097f16da00c7acd011f3a927b617d60be7502357ea7cb21566f3734488969&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

