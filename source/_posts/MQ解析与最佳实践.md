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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YYHSHUMQ%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQCITCdOkvOlDKnQ1riifNL4sG3B3%2Fgr1ilD4kyPQV50IQIhALoGYNJeXjZHS%2FweaaJRWTjO9cy%2Bn2YyYcAML18oNqKsKogECKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy%2FtGLW1R06ZbnD%2BYcq3ANr5ABLfWNgLtekHVBCgKMmFKwMHA4pD2kEggwdMm6OQbJ%2FgwBez2b6I54jzA11tu3%2BlRuDpVfCxeK2ExZPX%2BMomBKuPjp6tEi1vrdfa1pIcxsx0zKROW30YVck3hI3il7vpbrx%2FElnMCQhDc%2FtXuhK%2BIZavLW3%2FU6q48wB7dARjKbo9GbycJSAK8eX9aF6djWV0Gi31dbWtP7%2F6TszhQNS%2Fi5WfC6In5%2B5h5gOGVNbkOZIl8xzUCZUtd7ArGOElrHq8gofxBHPd%2FmV5gt%2FQkwCMhfDHrli6I9DeLsZgvUOMVzx4W%2FY1McCpqlUIWBuhAM4EQ1lCeM6qixiErp7cbBqYsjea6DwsmeTIgJFdKoSj6FvRhrSuWeWw4S5q0unqyp%2FLvMb0OXZp9UKi2YOEpsyISX2LPl%2FjFyvnhLY5xp%2FqNncKk1Sk66VVnQEIGo2Fe7bePg1fibpW%2BUNqE4AryZwOhi6hzwS87ZUBeB7lWKvZaZz2gfybtAZ%2F%2Bv51RmEUZBg1iFMPYFg6kqjpxLf2w1Ko8vbJ4g3rgZePcCkecZXbxxiqzgiaeYXlAGyiqlEZ5KPsUgdw%2Fr1msEB%2FJH8pUrcJ62hF%2FldcQEA6f9nksrR2uqRYtzefGKYy2XB0jDT4t7GBjqkAZ841ma4rGE0c%2BrGXxUsqgI0wUQ%2FUDrcXtT57JnHM1rSOTVX77tPLmaBQjKAm50VfuKSJH9ACsBnK%2F3nkgYDGJjHGFtYFdnsQ0wIcLwiSLygi1SEvzPivUrtdhKUCsDXC48TogwiAc0%2FF6VWOrAmHDqmGz134xyi7O0zTPO6Lh%2FGil1TPOV%2BDH%2FkgzRFIOMLe7Cti%2Brwxl8%2FHZZGJR5nM9eYWLb4&X-Amz-Signature=4d0c4e63231d77847c6cd45d6eed9436cbb53c5afcd463e9d020780fd86c572a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

