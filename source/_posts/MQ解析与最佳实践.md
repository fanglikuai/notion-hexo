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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466754E2TLU%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJHMEUCIAUGyC3HGLF8MhAcP2TxzL3J8ongq1U%2FkP%2BCYJv0wRaCAiEA3bf59nfsuqM2fx%2FHSVPndULXf2qiSUJfeaCuENfSc0Aq%2FwMIJRAAGgw2Mzc0MjMxODM4MDUiDL2FgBilS7tHOzbBPSrcA1sogG6WTrdk0I462B9%2F5JebI%2F4ga1NKo2kMSbIxegBkU1bQNL4kufXLdTvo8BAM8myzRIkvzB5tGprhaPhg%2FL1NmEybbvK6lE02Lmb4TaKzagPGZi41mZdD%2FlTOhBewZ2fO8dPPaNHTwrgli%2BNWK38pa01YjeXvCVyQdwz9hTN%2BO8N1tA7CoZZJCVzJtfqBX%2FmH54JyHVn6FSqfe31gSi6Mq3q19W6OPzQV7KbERGijctm9gpDAM0Tn%2BIfNJO%2FoRuwaBQBRqaxk%2Braig%2F3SespIWnGDDH7m0Co884iHpZUUzJv3gXI4YpZZVgj5cDJGJWhj%2F26NTcKtvb%2Fh6IdkuCoYN05DJrloxurykQ65VNZ5LVxTPJ%2FyYKnQxNjaI9%2BH0cdus6aRvYzAHgki6d0gFOP7Z99Fxgr2pBhA6CV2Cn9hZ7WZDamf2l8A9OB7RFnT8ypQ3259wcn66wU6dZix%2Bu4eFD083SXfayRzAAhkKVqchHWd7sS3e2Wz5axUJVvT26I%2Fg9UGY8FCKiswcMtEx%2Fl0AswAhsdEd4vwhQyUiR8gaBAPNabzxLXXDxAZwQF9GOE2fk%2Byime9AVnrMW0pVCL3AhNdDvX1W9aNT6qs%2BLd5gWiEXV7Kf2fgutU9MPzFrMcGOqUB2J1cW4No%2Fx7u%2B%2FoDay0%2FBaCycSBiY1rLR6tIeO6OloMcCKziY70hvkZlCXIQYmCBzmxn5oIqX7mAu%2FiQzL0UQUyi4p5f6i9YFtqLRdsk6nXkBWuNEFMWj2Slv6gd57zhu6kN2MVJFi8kvmsqFU3X2%2BNdiSwmHV%2FdccdlV6iLvcbFDQmmemAyJzHqLmqw4WnekonPL6gOHYnW6oVoxZFUzpMU4%2Bh1&X-Amz-Signature=741fb38b0da0f7ae19664afb84eb5d4280ff69914d1434ab2ff47bb45148f552&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

