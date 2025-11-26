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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466545WSYR4%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T110042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCTT%2FpG0u5j8RtOXUAkxQxxk5raiLF5rsZGapnMrfgc%2FQIhAJYBc8YVH6p%2B5rcwxTs7%2FrAECZyAjlx8HEWUnaB%2B0C4XKogECIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgziLzkXX95f8Gayal0q3APxcg7ohm7OhYm9PFGOudPo%2F4K5ucyFs45opBIFWGK2sR9a9d%2FYHdRFnhlhZYCSrSaMsyxtBL16GiF8RRXx41KZbdD4oopwToQbdqqF%2BeLnzo2r0VXY%2BdtND5PwLWsEBSuZBVDc9%2FNRKRRp1uDt5rXmGRe7t3bVzKHdXwc6JYcSw0W4F571kh7642%2BL1Z0jaF5YRXC49RUizihDJ%2BY6Wn5GmoiIwb9FlwRpLUxuBB57SqGG0t%2BRqvSMwi4Eu65I8%2Fa3Y%2Bmg0ntckmeWXwHRASlgNb9Dw0nID%2BMAH6ZOlVjc9%2FxpEUy8AuFvfJn7fpeLfH3%2F%2BCN90g4Bj7N7eWYH2GoMbZgXCHu6bs16FYw32fy8lT%2FycT8beTmna%2B%2FtF%2BLAmbztsQr7y7Tq3cW3ERltUhXE9TMpkL4G622NzDxzeR7MyOo4JLiWxxZVoD%2FxeEAYXE9TWkmUHMuV0eHfO0yfCSAiGPLSl2zfKKXFnn6WP0uulnWLJ2Q4%2Be%2FU%2BwlEMRNGZ6tkmDIelKKKfX4Vyv5EbxVbWqwe%2FHknmWMYMPnwo9KmzcRiHjeDqwqIZTlhB4Alngge%2Bb9njUcZI%2Fvi50XSYCiHrQLrtmtqxXIpA9JQBxMHVEKiOJJ%2BoQcH8Y3AjDDZjpvJBjqkASQpiyfK%2BaFd2gMfgGgIRWyCKAU77zkI0aR%2FCxcwmuLDssy6Jxba1aa%2Fll9KCvAahtNgepokkJHmz38CycL9ifxpzAJ8DtMIh732uLPGIOZx4d4T%2FcivAEbqvR6pajP%2BWA5L1s1rcBy8XusVCtwjfIOULHDMPkbnVyqnjl6TqqOnp0Abr%2FFuUh5oS0iZsYbMG3TI8R%2BU3VLAV15I%2FSrtDXbcQgex&X-Amz-Signature=cfb4b70475048d7afd373f41b194e30d9b5a4193ae272c8e5b9d6b6848ebe8b4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

