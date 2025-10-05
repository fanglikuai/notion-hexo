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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVTZ4AE7%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T170128Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC0EMtZ01w0jAfZLUVUZBQQP12JwbcvArN9oHHhVWWSjQIhAMjxg44UskBQTX9nW%2BtXY6Pa1CmInWLZjIZohzBV%2FaRhKv8DCHQQABoMNjM3NDIzMTgzODA1Igx4mBP9whf4%2B0SwmV8q3AOPbgX30BnQVpUOLVS%2BDnr1YPRXKizRJqGcMLv%2BzyQzhNcrLzSR3VYWO1LwvnL9cHxzB5fR5gyffweWervcV4xPaMySXUqlNWbKO%2BUe1dvMBjYV4FwQRefLZ4fc6M2v8XZt7iir8ApDG824yWi8kxznBrGKvJrCR8RL20jUtgxg8pcby%2B7Yf1lAONO3waZDnYa7JU86C8OCRctFDGAwZGlT3Kk5SSjhW7SPnr3%2FK1Sw4LHeVwBwcqL6bVHMHCd4Sv4F8%2FcEc6x4EjqccFOjPFq8sPLvXXtu1dBgouqIQlYskz4JgpZG0lo80A4ALdDHVBNEpA2DiZN8z9IP4GxbxExlPOFeupgz6T20d65HLNKvIxsOcgi5uS%2Bh6s0XXTfed%2FtlBGmxf3haXDI6i1HbliH0XJ5%2BtZTFv2Rqq%2B7TSQwnvmXfqGNDKOLp9BCj9Pyrhea6aUbgMVy%2FVfo6srsoD8Y5gO5gDcBmSQ%2BEtCfl%2BVe2iSQcCmLomuLXdeG1sLMM7NiWeChrhaxtJa%2Fh2Q%2BXfRtAq87l8IEgvHyYsCOlhm%2BuGWPLADSFhDNto5XVnPS2%2Bn8oou%2FGpJFQ%2F4AejqSgp8Hxik6xe%2B361cESd7eQLeVIsAd0NfEoGbMWbQllJjC7o4nHBjqkAWKVGAXP%2FxLEIn1COtK1IxxDwAsUbm2zItaD%2FiKBnf1PSg6dbaPV10k1FA7P9r9zHPsSNgHEqAPBz95QK6rAV6yX%2FZv0uXOh1AjShjqN4X5SCY2TzmggOpcy2pZ%2B%2FNYE9pgAj%2BtwLkIAAmoWQoC8jz67od371MPkCG%2B0Vj6RG%2B2JoRPdMI2buBMFzKi2tvo3R21Wf%2BQqbT9nm%2FZ8dKECEE3j6LTg&X-Amz-Signature=b50b9c839bf432934970ee825cc8d794a2c6ef43db97a626d0448c1355d7f0c0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

