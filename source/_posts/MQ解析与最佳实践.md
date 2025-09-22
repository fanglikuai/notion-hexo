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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TGDKKWNT%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T100045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCjtU5IKJv4lPPrVodWkrQvfOC%2FthF%2BPPOvqwuTZVBhOgIgWycs3Y7Ny5hRq7NEYWhDA7j%2B1dDvFgln2qSB5iekywoq%2FwMIKhAAGgw2Mzc0MjMxODM4MDUiDMU31cHgWaVsnsnsUCrcAxEkcL2xT0z5%2BUxjrUDK%2Fkbd8udse0hoLlU2yJRp5aYhNbsF%2FS0soqEZHf1yfj05ogOkcx2uBjDyChzMBZBsVfMNMLHNsZmNm93GOccyjjTYAxENFEHRjyOH4zBw2vaWiKzpiaNqqLtE3RtK39fb1s9hcMz742v0rzDTt8B5hPqxnc82fUkNkvCkFHfKrQjpwNtL7FQzZBjgFoM1DfROvyuOGC3PMam%2BmwGQ%2BZjopgLDZFq%2BKmwju1KLT58Pd1uJoru%2BS4C3dcSahcgfUjpuq84dI415HD7eeYYl5C4wh%2Fwc1NiG01wwcn1nnZ2UTifyP%2Fs86tIVJVQahOP1fGUSlIRkIBL1Cae3zv5FayzQgNwAEu5Hpxe%2FDa5VyzlGYYYxRpMAFVmC9y3%2BAk7KEcq1D4hpFJUpZ8tZjmLAB4ZuucUG8EH5VqYVW4sWgdaftB5J6nFqGzRaqt%2F6tOCnWiKJfFHceQXVAxiU%2Bdr7KnTTYmQ1M09IK9c3tMDz1%2B0rAr872K6FJDi%2FfstmRANyg2%2F%2B4xg6Gik8aex8y4aO%2BDX6vPsKrz88w7aGYIPkJ6SAW206oBMSZXJtqVTe8UVPc98ff7Zw%2Fm4EZLKQ4mBBdLS2oSUoPEszteNbmGthl75sMMSdxMYGOqUBYrGk3elpqGiVtFmtOycsRE9fHwgUL9h24YIf4UL%2FGPoJEqPjexpRqeva2FTiV%2B1%2FCBE%2FD3lK9yU0e78WSWxZWqAqrZr65i5mRfSZhKejxAWr61CUXxu1I%2BoQOUwWWK4EQu5gujd8iRGEBFLb4LjXlxL6IPNGAAcssfIK07M0Zt43Z1BzKePu9EdX25cyzlBpM02C8saIPwjFdulP%2FtQBPVh0pn%2BK&X-Amz-Signature=890b2e24af0116d68891ef9f0214e227fe429486e7cf5c0e78ef5dfd62e82c04&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

