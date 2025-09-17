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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XVYQZ57V%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T120050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJHMEUCIQCEEctbmWQw2DmiRZKGHjR%2BxUAvZZCosR60oHWFrTylSgIgDlp24wWlGDy8X0kaVvZIqPBdfU9GbN1DFKRpiZS3TWYqiAQIpP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNAQHdpU5EpWooQJaircA%2BeKC9lK36CJ2NnDqf%2BquppdmqFnbshpLp%2BZKCDXRYXHGe7tOY4CuJ6iG82GxnqIFdjHGJ%2B6h9sz62IKGKewIrK2Fp37ApM7NRu2iS1XIGC0cN%2BXvOI6vg6HMKLBr3G7sZloUn7yBQ2xtzkdIdsJaemEXA%2BE51u62k13XpHZ7ODFNE%2Bsc0YlkhW4CqkyLqCUpJYOwBOzWEvMmXYqnwfJxD4WyiGV979La6lHfcJY3UAMN0%2FfIfxgWPZgHlJqKvk7U0yf3YWEZkOn9KQM9ZQqZVc%2BzovVdsKus6LZXB3k1bL73n%2BQ2RE%2BPvUFuLgyKA79Yu0vB3CZqie0ZAs48FCSA%2BEPLDLfmzdak7WmY%2FVve8%2FqCOMo9%2BqGGjy1AqSoGU2RCGldweZ6o3O9bcoxkKzdgGMon8gnic9zMBK356atGlLYYmCK%2B9HQdJduJF3%2FA%2Bw32t%2BrfqExU46uQje8uKFGq6LjrAa5YwBheFVP8wJG2t5cY74PgCUIuZjnAgQdCejjN4ndRTIUOfbziYrc1eaUnYysRn54ZuKuYvFaBLXJL%2FOdGtFnItO3Tl2caf146CNq%2BtfKrRb5Mko65DhuBDV5JGLFX41P38KhE081OVEOt%2Bt%2FfN5m5H6TjK45Vmd7MIyyqsYGOqUBw26wc2PYEs%2Fjlnftazoz3jugC1j2JpQQphEIyBLbR6dPh4RZ%2BHWCm7S%2Fd5f%2BlnIaUOhtJvWwQ%2FKN1XF8H54jROU5Ay%2BNEuAkwJWdG5P3Tbf5EUY53v8fLCGBYbZSp56UzWluN3sPROOHYjys8a4c4lT%2F8UCpTDV0JNjvf8m8x1mpBLJIh6%2FPPz3goAnqkc4J%2FYnT%2FHa1Y2qwRuVmSaVBc8QYBHBN&X-Amz-Signature=c62477f414aadfab3ffb04f2302cb13a29dedaf1cad1019e6be63e4e64e316a7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

