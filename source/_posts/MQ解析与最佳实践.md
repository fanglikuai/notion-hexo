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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UIIC5NXJ%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T190048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCDDcOGDLUiViNC1%2BLr1GMCUTxWbN2sTFxx9fI5Dat3WwIhAI8SHORk83VSXnK0WKpMTU%2FfFbiPpEgL0V%2FRytok0%2BGaKv8DCFQQABoMNjM3NDIzMTgzODA1IgwVddjomAn91FO8eXEq3AONk3S1Hf3rPyoVFKQddcvPdegm66UySniu7JwByPR5XNhUc2m6CsaAC%2FH7upz0yZspMjF1cQjvfma%2BM1I5NptHdL7fE5LPUzgj84Fo6BEqt1wPn%2FeR4B451463Sis43ESQhIKxYMIgSAhwYkwPa3CS0fZ7SWpdv%2Fm9dtDE%2BQEq82DqfqXw7vGd%2BiZJYMCZyB5G2mwe0%2FUgTn9qBaE3DwMGyEgeY0kYVHpAeNf4HOS5TNbzaY1LA67wYDJ7fpgRVmUXD5DRObJDdTSgrba4ksPNO3XfEgsHSte9mLPpznvvAivP%2BvZuCKQ7leiVhSZWa6XChly5MEimq%2BTmOTo7XV8pyrMy9gIUrYfORSyf12QcJJpgP64CBB5%2BWEeJFQKmcGi%2B5B0evThjeRQDV5kuE9BZ7l0KHoqj%2B7VryhqBObGUzg2qK1vm5HW%2FI8ALxZxEc1bMMYUxaH%2FiCd4ZXG14AjDwTZXE5xiA4euDIGXXZ83TQLYpSDeFSLFXQiORXazDDHZ8%2FVOFx2mVs0J1QCCb9Mx%2FM7pYr8qCp4OX9JrmGydHAGAxWZ01WnssMu9Ul27o%2BFUb1Yeb2qwlJS2ZRWGmn%2BUCuSKcCS%2BL2qzTSK1TM4azNsYe8WVr%2BbT%2FYRf0oTCT0NjIBjqkAVll3gtRuz676hxqNMZo1s1osL4MpCKEw0DXzKeH%2ByDnxMXVE4%2B2RXvgmjemdBWKfsYj2JdwC9uJ1do30F3kFAkphxu1D2PW7Rg9pabmFag2c8ceAd7wkDS1VVIXsk5eJibc%2FIvOUFOESnLFVuzQ3vFJMDamwKZDr%2FQzMwTzR97NmEZBnStPzTptHmMACsbVcrdC82kQ2C2%2BaR%2Fv9Lm9vshcnzvg&X-Amz-Signature=d992b194aa8b8704656859d87b7b7c865758c659b362249c40b0d9d2ead4adc4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

