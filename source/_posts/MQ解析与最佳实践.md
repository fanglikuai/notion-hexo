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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663RSWEFK5%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T020043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJIMEYCIQCF6ql50DNyCgEUTt9XBPbBCZXpcpP6molHT2CteCxJcgIhANTbsU767DSqI16cHNj0VavanwUZvCpp0DNklmSdtAyIKogECPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwh2CbnhTGIeqD%2BJSIq3AMR5a3BCs4pYC4GJHWCFADY1iAG%2FgrR6Px4ao%2BRx0ffdBzsu3ljBjF%2FAQspkPsRNGSEf3yEu%2F6nmtyfxqgJPFk4Q3UxjIjcFrKOI8dIjw2pgCgoML7kEiIMXriK9o%2Bd%2B0Pjena8UY0mrp%2FWRFjy0y1n6NM6%2Bj9NTi23jPwcMm7Sgboxac%2F7U66cTP6P%2BDfDu3vxIxXPJTHdotgAwo1y6VaOu8UArBh2vfHsujFenoR%2BLSwPS2OYwT%2BWAG4pd7eR0PCxHvHnQVtG0vE6Z0OapX%2BplzyD5xPejFiaKHDlOfnZV8Cap4YfWiL18oFZ4Phlnj3Dtrp55zAdvgKHpUR0J%2BsTz1ahx6Q7qijJmwa8WaHeEdBXD9eKvkIOBKYkozw70denpwep9k%2FRhB7hjLfGOOBCa%2Fv9TW3KQAl3aaphMun8fQjoDMVGLKq74gYZWhyY0A2CfTkbNcwnsf%2BcRH6mEVgWgGg705CNvOHVklsvQarXWk8hg1fPDKzMZpZlX2XlLLXKT%2FR7fCw20oA8gCMXo1Wdy9ArAPsbzqp6SiNnjW48GNpXarA3Tsywd2ZH9L63gamYOS3Ugw4q36lRHluqmAH66X6%2BN%2FKYj7Xjdro1UXnBEmJptm38RQkBxNsn%2BDCX5KbHBjqkAWnyf0kxFVz81Ll%2FvzwCPo9gd2G%2Bn229ajCHyXK5RV8jH%2F6zqC54I18GRWYQNIdzQqdhUA%2F5pfj8xN%2BbS1PznrsxItUhel4usOUZw80gGXtPA6iA6Ud%2FrfmVEoLiwLib8%2BYMIejHQKW9t0xlavdBMgsPL3FsXt%2F40uQaQ0K1V3hzf7mc2%2Fy7mk0DtuzBiQchEVJhzNBp26jJdoYTvfYe8gl5v%2FFC&X-Amz-Signature=bd271fa24c26a583579d8617ef7f7572a0dd45355e7da63a21d07af34575e1b6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

