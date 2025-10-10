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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RRQQIWW5%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T090048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCIDwPCedgZSa6LDQyZbdPZ%2FZlnPxaUpspkx%2FIEi2CDwDwAiEA053CziBvsxupxkkWvviwa4dcsqnoFf0EyrUysLT5hCYqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPew0q60n3QCZFSXIyrcA9ljNMIVlgxb9NeC2z3uSfmMYRmAKYiabef94kQZDVDGHL2fMwItLq5%2FzWo%2FYf4%2BudFee9ABNA%2FA8So4LDlsm2ki%2B4PGQ%2F8B8W%2BnmvG2leyHhqUmEdDYqE5Ckj8mX2cghGGj4CGhmmw6vKHJIeoHYzHoH7Mea0sp0pckEqzUNuvyctBfAFqEg4K14JdygHMCJ9q9wwfKf%2BGeSQaiRKRrqniSzBQuh2OCWk5vtPpfWXih1lhMChoWj25GNiQc%2B3dIglF%2FlfpDK89sl2vxgHqxt4LCcylpHoEjs4%2F8odxR3RfKNMvVNl%2BZUmkJnKp4BR4Sk6z7OLhlkl%2BGzHhxU7NwKwXcuesAwxrUQU9t3oIe%2FThHzaGwFhs90TV66TNYjlb%2FC62E3O5pDY5AatpO5mZHo5AzZJlGuHBjVCTw%2FuKZ7fBTqocLpx1SFPLDn7pnxb%2F5saQWfIV%2FzOfqBvNlTjkH1yBC77yk5dTAd4w7tdaNRz78%2FIHlQyu7vgMYLfEKljNfSJGCRShCzwEfD8yGWXz1IDJd5L%2B4mlm79u7fXkbMKT0bUWBByqKz4SIPtD3DyuftcMkdLl%2FHorLCY4YRCLUPQW6GNry8tSOkzWGoZCViHKntObMzB%2Fy60oCZyFNiMMr%2BoscGOqUBUVwWj7%2FsXo1CkVdrp4gJ5fEWbJ1EOdP34bMjNXw%2FNnZ%2FH%2FkZROg55tX7Kx1FdBY1m9zp%2B%2FSHIX45XffjuOiBVpR21whtW5xYWsnLauPqytNWPUhg%2Fx%2F3quFh9GXlAmNgnDagXMSMIDXnKAS5Phd77rIuBMzgxAiYkbj2QdWoxIl2gOFSWRjfjF1O6NZXVB5ENKFFseddEVWYCIuRhNr9TSMD%2Bz%2F0&X-Amz-Signature=49dd9dbf1146df4f15e2db49731677747083e38fb710510216c0265e8d958779&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

