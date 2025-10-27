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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YPQDMUME%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T230036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGwy2o5f8%2BBEqDHRM3tUbygtbFVvr5c1Pr5LHa4ixP1WAiBW6gUpo8gNOHCFTCA0Tszn%2F3vspHzlQ3lH3CaOihbtCiqIBAiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM89kTXxADQqY4Kl8iKtwDK6VMFW6sOTaQSAccHWA82abR4RM1DXB8esoYfWTFgUQOHViM0NyylyDT1Gvi86jv1UYCn0iFcsin3505nopxgDuBAh3AFq8lS9mHVmOMNaPKulQt3guwu2U8yauA9b%2BuEQ46edSfLk08KLltxb%2BuVTFxVWTuGamShN%2FcmAori%2Bq5J1V1se5arPu%2Fj9yecw51lnvDDoCq6tjCGn0eKK0G2EdC4YlInTgKAEHMJPc5G%2BdQvyC1v6aK%2B8blrhE9ZbwMW6FxSUYxxwzb2MQ11Cfdz0eAErqOkD6pkRU2HY%2F4iIduZd%2FtG3WmhcLFx68GI19JPPyy3Oc8ur1D6w%2BkPBcrieFJwMvHU8SL%2Bb%2BoDgE3DGvdTViCGwr5tN%2BULdOTyg6eixDsiZo5Hd0Vz0c75ysXN2z2bEevx1oC%2B1tmvGXGT%2FczI7TEBYrDY9eLHyAwIAiFCTCpKM2bCOuRf9NloxK24gityex2uOwE5N5YY05fA8ra3Nzzb6iFhZiLNmvze8z4MScrR4LADOu79275Jdd6gHBGDeS6xXzSQZJZwgCqGUJMMb6ix0Ua0kltYLFJNjkkb21sh4gTSMrw7A2oGM69C5h%2BUD8ixYQAouhODtibx%2F2NTeLnA%2B0ZkVhNzv0wud3%2FxwY6pgECFfLayoZeQp1GUobVLIVXhazi6K%2Bc5H7HQP8lOtbCr20plL0F0UCpifRaP8KfcJ485rfLKLOxq62OKznFof7yqB4Rqm8%2FEIY8bQwOsCz7o1gGXVRU6AMnja3sV7yDpjKmQQ28ZfAzPtKVGguI%2BhXUMNe%2F%2BPMdL7yNtdV0Dk3vFALU2fpXHw6mWkNR9eHpUq%2FKpnNyaRh461jL9f0NoMi%2FhbWmAM3t&X-Amz-Signature=8522377a4c36213501447af3c274342500c814b80951557a1b1612cc4aa163c5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

