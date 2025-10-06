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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665QCJQBWM%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T230037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCSoEbi3tBGg%2BdckLTt5Wc7zFJIq3eBe9pCfgCK9L4gSAIgHA0F0yc8r%2F%2FSG%2B5eBD6Ubvy%2BZRLcEaTe5VbttKRYXyAqiAQIl%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCQlli%2FII11nKUptJSrcA3kr9mGMqDR5lkfFfUV6EcrL0Q5ticHB5%2FbwEyrQJIqk5KvALapevq6nnh3hYrMCSE1cUlQuUc1Qe6zuW8E5N5yGTYp9b2bCJShM2sSpGo%2FnyjvpRIzZN9LGH%2F0jZgebFlI2BFlmqZl1tbiCPW%2Fb63F4V%2BxdsG75bfAamwuzboa1wh55ynFpJ%2BaXiLnzZPL1r0r0tNe7gd2c0oqRuEC1u9QKnYPhS195KNlm4OQDzKFVWSpxSksKD%2F9%2FVZzXUVphyxDZQnsMa6hkdAKHaHXYMc7Mcz6dRJSjuYMuzAZ4A9WGY76zrVU%2BbnkDM%2FVaW5h0Zry%2FsPciQuWYKUDL6nvIKIvT3vabec%2FhkLhvuxjFfQcqQbwk%2Fbk3fY%2BCs0BXHjOcrDABVyhKItk3GgNEkd4ZyNY4U%2BzaLMK%2BfjUR73iCDEb%2BnoVRjck34OmzJZ2vgUHVu5t2aYNZPkQeD512MTKcZTkwsmB5zerY05oV8RVWWQzcCLxLJlqv27581CpecNXIx7HG6xoCpvS84YTnheaGMfYYnftn4nz0FquDg4X44NuKW8KZZ3WhJmtg7rt%2FcgiFU%2F8rhFGk6Vrj3SU8wpz1LEor%2F2GuUIDMyOq72VDjrh51Q2akHI66QQpJCjEfMMn1kMcGOqUBD8O2mNYY2vMfX0baTduRzO3PWBxcfuNXKgyVXiIsOjt89RiRldVxBuLJ2wUspl8dwKWh3p21Gy4oZKKrysNOTjnRpeTt5ENMor%2BqPyWaCWlEys5N4mBsxGZ7kSV4M%2FpI7zdE1vHPZPmwDdjuBwwYhz4Iq2wvhF6mftlO%2Br4HZ8igkJplrcFmUAV%2BCSFPjMGNDpEVqEVyjzb84T%2Fmmo%2FmkocRh3Q2&X-Amz-Signature=81ee3169f449af011b9ac1eab03b9b19dc56d53daf4f0d17d06581bdb22d52d9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

