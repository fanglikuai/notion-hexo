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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667Y4QWTS6%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T070052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDqRQHs%2BJigNVpSOzt8iA%2Bn8NKQoQpequ7N%2BRAbSy6gLAiBP158LfI04127Mt4O72d3hB0TabfwK%2BpjY%2FJFoKIP6tSr%2FAwgnEAAaDDYzNzQyMzE4MzgwNSIMufgMDwCB0%2FVJXwkSKtwDx8NAEWwvU05GkNFUAT%2F1oGKmmNGeArSmgaZ10YUaoDiukQPiVWECR6ppBQhoVza2MioQxpnQO%2BYMpjbCmHfPAt1E86XsWJ5xtvJjfe6qkAMBozkgMsTWYFcpQrFj60Hg82GFcQM05qATu%2FkFDx8%2FmeuTxh7fthQfV8arZTpX2xVup86QckcXQRULV0H%2F9Z5r%2BISOJRgyIQPS6N%2FO3cYF42G0U79hIbVkTvUWJ3aXxOmGNKCy4wAAbYE3t5BP6YO8XSGJJn4ofhBEMEmkx65mRh6BEJuKt%2FIoh%2FCzyv4zzjBac1fEDARZqgrb%2F2HobKFwfUEftZxvZs69mY0e3UwuCKkW5DNjq6iTlAowVOcvWCMlgAn29UoyAJBsTHp8ai8Xr92xehLWuIa%2BiLDKA67dLbmgOCgWHvfWs0ViqfyIDiCQ32G04OG42qgWrU2NU%2B6buSgj%2FE14GatnyQwwktaHI9WxB1DDhWn3eSvCuxrVVB6c6tNERHhUUNRJfMSb7txnEXL4w%2BIboenDxdfUUyHO3xCyAeCFJAlAJ%2BWJyvlThro7c1GUC5xbqdycpNCu4AUQoF92gn8xsDGF0MoLSR2Gc5wxqgdkByZQa8tJRLaX16RP0eDjeOZyA0mnl%2F8woKP4xgY6pgGgqF7%2FqfwKwk696lfzJ0to2nC55A%2FdGY4HKe0EXUNft7cLoAaT2Y9l5T3iQbb9WBNe0lMu1EzkT9RAv56lzJ7u4maCnTW2TJXo0Cgu8WtTFJTOLLJ4bjwdTrmwDpd7xKlyGDoSa7DSpqwnrxgQBdeE4IJ9k94BV3fkN3H8NcJQPqks9Xx5vgXbATNnGl1OM0ZueS%2Fw51F8aYbNmzXARpyhqCfLV2K2&X-Amz-Signature=c63a5d03b5e551006336f6c1b0899ac5a727d6ed0c531dac1152dd9dbab7f590&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

