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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R3RN7SFJ%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T180046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCID%2Fh2s6UhLiZqFhxsc1X8nlBqD54jBZ0oroch2w3ZE%2BrAiEAq0DZCF8Mw0RxogtAwCTuBglfp%2FRGdsM%2BlogGsXa1mjcq%2FwMIUxAAGgw2Mzc0MjMxODM4MDUiDDTIfoUnflaao2F0zSrcA9WnP3JkkFAJpXv4xkSrsotMB4PJ5X%2BLHdRgp14HiwIKbEJWyBTMWZrNW3qUtH0dU%2FYiIUPcVdC6acCd%2FWSjJknwx41tLBHHrrDnG9hciR8kZd6ghVMHRYXPOMdl%2By1x1jIZXFce5ZMGNUQ0sqHslDmA59xyQX3RK1ux6HX1eRocgSceGLYBfrdGAGk8YF5RqQWSQyyCfXZVZl3XpBnz3T3hLa%2FOHpEGIkPZEPCUqhAEfZJymZZA4Mr%2FzIHvU9tZQUyBdrsOJAT%2Fx0tZ7E%2FNjWmPoI9FFoGL92hxynXVGzD1qc7Cy20p16%2FW5j%2FwS52f%2FGzFlD2O733aou9a4GJ1wiBId%2BYVa%2FiDkdhBoaDGm0%2BpLB%2B9KckaVXxx1VYgMWW77vUDs1G2vRZ0vtS7Ck4kYJBj4odSZFTowcRRVe8a9TStBh2qvThd2aP8Z7nfgcJSi6pWPWb9Pb%2BMgYgtldBgEJ0lk2l7qB8Mejjhn1EAnm5o2W2rTZR2JD2eNvR5vLOVh3tWSyfPyIihJmRQu%2FwtlhHdLADbVcuYJKu8iaNd4BKQuWlVnh6ZmJQRn03s6mS2R%2FmBbAIJhM06A%2F4JUyOxTfJ1JIL6fZSRNUANzr3E5UXWUE5L%2FM3D6SD5sieLMOCw2MgGOqUBQC6y66u%2BCehS%2FfjUR9RQ%2FHEefi7CdFLcZwTeXhXefs7tptoolpA4v7iDiY6lR%2BBXmGMpGnk%2B3mE3K6uVYMlxV44UOFaCYxRK2a2gKl3sXR8uQ8OeJczhs7RGVFhE1WZbs%2BgucHaXr7JNoE%2FpNjugBV6tG%2B9Wn9MkaOSyj0p%2FjygN%2FehnN9zTArlv8jaR7BnitrEiqNjQ4y3wtUAWcH2hLSR7jTxS&X-Amz-Signature=51d5a80b4d37d6325c03a25721bb3bf155a3e02edd09dade8021499c6ff97684&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

