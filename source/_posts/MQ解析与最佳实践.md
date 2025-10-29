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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UPVR7K7K%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T100041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIQCHHKjwtr%2BuNnZO3%2Fhz9N7BRAo8jTnznlGPywyM2xg3dgIgBNf8BEVtd2biJ4hjfbh5OneMTXRn4FYNViY30YDi31wqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM7TK25QDzpuDtng3CrcA%2FBg%2FPlijp2fFBeEGt%2BDG76tM8mo9sHucJjpfoVYHp1rbMOO4CjpZ%2B8Eu5dKthAFOKe1Af7ayoUd4f1z7Keb4mBpwkNiVUTRxMWFKMghtpAp2SdAjUorPwij%2BRi%2BkJgg9ymonvmlhyR6BImBG0778WgTEyuEplxK1aUBJSel3NZvpWLLCgWw%2BANlK7AekP02b19hRCR0EDVLHEKP7PW3sg5WaijBLbKTaq56Tqeuti2FYnQxWuMWSEkuPM56U23b9auvu3Ad9aZkhSHXKG5sYcbhcnWqC%2F7K%2FHKlGdHFFiV3BFTzPthVVDrhWOLdcHZnkA%2FnlRBe4b9YMQYB2pzEwr%2BpHoKfjdnIEccCy4zrClUx61D2a6xFCx3Hes8Ev93%2BF7zZkq6TjDNzex8Puv6nz39EpSvc%2BFt6UyTJuLdSKTSm5PY8%2FwPuFeMubaLTXh1yCiPB6imiIo2k2DyDeSigTyF8o97Py0Z2yemEOUjOJW%2FUkOKe1OHHT8vTeL8wKtX5Eie%2BSbRZpb%2B5zRMLTQn0mir6HJ98CIhuNDjYfnzMuopB825TraC2DEIpBt47IdQxqu4G7jYD2cNOKo9u6GMO538MTk%2BcFHEJ4w9eKRajhjMFIQPay1Q4HFbou1iXMILKh8gGOqUBcSB7NaI5mb4HuFcZ265zPHZIs1EewyJ5BPO%2B1VX2MOEQZUcWqN3sxLslxrVjgxgbTrGh4PvGsNAtaR97W%2Fp2k2NMymVHOsNYO1cwSlWxUPRjY0e7vCEK7UPZch5%2FT09SbndaH1Pi%2BGtE%2B6Pyz6nB9LuYks0NQg9s5y9AULxL2r787ia2EV5luW%2FWcbzUZtWZlWOjQZzntirgEcsWf1%2BrR4o5cZZV&X-Amz-Signature=8edac2e50697d78518b058e35cd0dc128f085cbe69c0091e1c8ca041a3f0dc1f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

