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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WDOUUZTH%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T090047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCB8jEHz%2BVKmrYd%2FeYPbS7zKF%2BZc%2BqrqtszoegyB7p4VAIgC%2FRaymRR%2BzGk5J2%2FxgDgujvdxiLV6ixtz%2FxJfJNBrjMqiAQImv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJLFeOlWwtx50h7lzircAxJx7sRgCWM2YyxulzXR16YBCg6HyXNaRrFy865aQbvwv99ps%2FWAwrXZR0GvV6ASwA42zQh58g8WNkLp%2FdzwI%2FBICHzLV801UJeUR1S%2FEqmenkvZxYlu%2FOmRf86aIwfkOl4Lf%2FZHC%2FwzZT1DB5TPpCD6pBdMydDO8%2FjhMyC5w%2FC3imeHQINnRiy7SJ5esF6hsvfHmggfiCSTKWeSv4HX14KB1p5HdWkWo0HbDpDe2a%2F1VRDm4Jnr8Sex68DLX8g2o%2FF4gFSTVSzy8O0z8O42gPBo1%2FdUd5y5lhGIGE%2Fo7yI8I2gPfcZYqlsRWqTa9cOoAgSRQ82yCYPhkT6wF7VddKAiQo38KPRcGrVlW%2BjIdGJdOyrdjGp3Z4IV4lto1C3yJP7lnKyaqo5S4F%2FZCX1TahndC0Ia4OKJThUcNqI%2BaP%2FDNOA67lHjGX6EQiv0BkbpeY3amrBAkekl5qclX4kACe48KASMnc5OTLVzufngdn3Y%2Bdk7exVeWMlUla6X2Azhqj3mQ%2BHpFGAf8ji9SQLi7aKhR6bj4L14Gvg2ki2Q%2FysQchWaMpycUGFaVWO6LzrAn1B4pfS%2FVNBEA4AH93C4VNsF4rdXZuLi4yKehz1ZohGk0Wn%2BgxCaIjrz%2BRcAMOGioMkGOqUBqJt1zm8s7ll6qaORfG3LegVtIiLEy19P6boR6Jp65B00Y09KcY0YIz0BzWaC9OW3FyhSkh06imSyDhDprEx2sPiFRelb4%2FtlSUaoDvv2E2DY2OTM4uTgyureqd5vtU%2FTUCc%2FaNM8SdJeD%2F%2FCeY5yAemBgHbb74lFR%2B6mQIY4JW5MgPFh2ydZtAt%2FWmJv9NUC%2FuXyosfBzOG%2BuHQ66wC7Lpdkpfnx&X-Amz-Signature=bad0435264791cf0d2ee3eacaa6bd69d2bfea9c96f1b723d166ded8de9f5c31d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

