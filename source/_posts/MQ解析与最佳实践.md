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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46633IHO723%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T200036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJHMEUCIQC9ppbhkp1Ji23GDgcIdbjYcTcpPssIYfeM0be8FP5rigIgdmm5iIqvhwZrYCcq1uJI6yIBUZGZ7d0ucAixKJA%2FApIqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEB3PFbK2XIJOvT4bSrcA2I2idlaiOILbAaYCP7zFZrkoVTWmqJ59llNm5D6Kcui1i36ocWRWbDP1nVLjm8NFXRTRjC76K3LkDSf3iJbJF%2BxXsZVlg%2B4f9pZrilHh54zIBufU6W43ZIQrC%2FWEuMdSF676Z9F1mf5HqyrJ%2FuFij9AFgVGZLC%2BJXKTbzoHlNVqw%2FLMNgFJulekVBdZ9DQ%2BXEYKnz6FImKVqGbFthiCEhyBibtVxRf7VwOI3nTpsrzCnYcpITFXLWuoF98%2B%2BEvD%2BmHfjGKIfY%2BHWrqEXIIGg%2BTLLE%2B0tDvt%2Bejkm8GVFOcxbbJjg0ggYIpmmcDIgHdCvTZojrFxpEcPP6qpdMgpQa%2BB3FHAE206xC1l5ZiJC36RcBLcf2QUNXd%2FPp%2Fm31kET%2FWG4peJ4T1tKWqND1DMQQvVAhCVffMCfKfg24%2BNCXvyR1HeIq%2F2UFLZC9DxVJ48TJk5hgCCjcdcqTexuw%2F4mVgTDtY7PN1%2B3M5EM4KlJu%2FPpRtDAN5k7qWEwZVPUo7r0NEEzfqfDDf%2BEq2u5UqsqxYKdEGRsHiii4MOt%2F5Ch%2BWjjrVHMYp1unzroTpv5GByOmMXwjPsiEwh0JSh%2Ffq%2BSxbreHHlEfiV0anCeXecq9j8cU5tH8yAtqDYeUliMLbY5cYGOqUBN78M3hagkmiLHRtG5u1jBsPUtaUc%2FtqDhll0nBrsc2OIcyDvbzILT9PzW%2FkWRCt5bQ7j6hggfvoFvvTxl5RaUxJXR%2BohYCzM1uKm%2Fe6ot2Vsxd%2Bd3Lp%2ByztCJTLBrmYXr4OV5Xx2c8C10TAEvEeZ9s0WuO7mPqPH2IRvP7ucqE7V1yq84WAlbtrV8Se%2F3ltffe0kGD4KNZ167h%2Fu%2BlbYDyMYmv2R&X-Amz-Signature=86b189dab9ac96f3e350e75b926575ba7fd702ff8fd5b2ad6d0f14e0b8e012aa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

