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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UDBEFVER%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDw49VqDZid%2FY4w3UOalarWnTxo26SKVgc3BEHsPHxSpAiEAz8HjfoH5SvfcEWPIexvC5inV3h0iy9koYU5bzfbs7AcqiAQIgf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDfvXPFXAL0hOt2pZyrcA7HLCi0B6IJPRtXLBQSGHRoiVY7nCN57fWoeBVyEXW8bxN%2BgurPMC51Rst%2FH8DZ%2BIC6GJgMUvuY3TnO8yokQPhQdb1qLVU1xSU%2BnBEyMX4tEqdUw5xrdr9VaRkTyrjWDfLX9u%2B2ooH0%2Fdw8IPLBpB1%2BtylH38GAUXTKm8qVuXd4fOqvdPIqcQqYMa3z2d%2BaeumYERZtUzphElrBzQH3ohmz1zPl4jYTgz72EL3FrPPZC1vzi%2BZAtw%2F955bFvvx%2Fp4pXp6BGAdnPRYPa3zq6PgoJ2MHCQ39SM6Z34zo%2BECjoSF26l11Bx3kcbYuSs1R%2BgpUeHiGzvWe6mJipWusCy2TmdjkAMm0WS3i9nrhrbe7I8rvOKUUx0qIeXL02hyEG2EJwIAIPNyeAFFKATDq3ByCs2fisjz0sHod9UKTd4C1roGmio0z%2FVNqPLDCG4cfMdDBe72Cd5QU%2Fw1Kc0a3YdenJJyR%2B%2BvuKsz8owV5LatVvooQGFJyxkAlvtRmfDRArlFbKKMTtLTMLQjaIKVytHYtR5%2F%2FdCZYuGYa35sz9O2Lzp7eCfZfWmK1ffljRHXWODIeosE2dWIV%2BDfGRaJUag5UfxAzDydSmDn4rURs%2FvZlzflFndCbGp%2Bq%2BmgfhmMPr%2Bi8cGOqUBR%2FL6u7NW9cK8CDHjKec6umIGm8S3Wq8Y4sne0A780uswrYrKSfuvUHFoCGVO9DhEHbryuuvZVdaKknysQIE78qxTPA52BRoGSQdl%2FTwzMHj4BkXJb2Hk8vRWUFGfqlLEcA5cvDhzn0QjZVCaLYmv3z1HRh1dO%2F0E2Mx%2B41xzVc5bFvNhpfnsnWixXUPjt408SjMtoo%2FtzKUWqxGtPFtdfjb6jEPe&X-Amz-Signature=add7b139b95579c603e3cd5135372ec3acfefd25cb4588989eb9a0b2e028f801&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

