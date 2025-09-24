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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y3LVO4WN%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T020049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCVIrEjQCVZ3eh7wMiu5sMXmxV%2FfKxOiG%2FQv81dsO15wAIgCsBGEsVoYE0lkBq0Ue3o%2BQJ0rFSqvthQobbW%2BfNbuxcq%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDJe3UrzQDreg5e4fmSrcA6yZzOkFzOegWLUtS7YeeoctyD3iV%2FQsrwZUE%2B5sHn2QsESH1%2B%2F11bqkkDxGeqI8z%2FN9cpH9pplJVmDIqnEBL4eloLhrso7y%2FQaTaF4fVui%2FqGrSo%2BwChiXvExw6TAPHYhbx4WvSty%2FGpJ8G8ZJIqh57wh9Sob%2FaLHERjG2axVeiTu1gwBpb7f8P0L9x%2BTmHdQD0d0AkGR3c82HNJrcqD%2F25OIV2XOmCPFWsbUNEknXB817a4S3rtRfG1qSwmyAbeyDXqog2BwCLUJehwbP%2FjvGUg7NQxHaqwgzCxLMrvV%2FTdV5WSNCNBXQQFJz7%2B5UtVzU0l2xwYrsW3qgFNpLvBHmHb8UWOMD%2BKxzky4D2bxvTa2%2BU4pLS5ZLlkAAK3u5c1%2Fz1kvQvC998wRU4%2BUyhIj9YstkSPiMS1kPv0ddODwiHgtUKArYGhO7p%2BsGhMy01T9KtnmWg%2FdyL%2FgTxHort1uHEpGvTi4CfllmwbTuf0LIjS0KacQGUFKqUnXf8KmLAVUpMan5Y9GueD0CeoE7dVfFsdgta6h0HhsN%2B%2FdDIy5QhFAqL206ffaqsEfBNt8uSH%2B6SPn8Q1Q%2FNU%2FmrM1O6I2e03coV7qJxhL%2FLXZ5YHZgRFfsCsZuwRXVPr0JTMPyNzcYGOqUBFlVWpnHLWUcw%2FY2N4LW4MgSyA%2BkO0q%2FxINX3M0xy%2BIcLtT7HxDP94TACREg%2FXpdoS1OgYp4cQd2TM2q26mZ0Lmf6LqfW5gYrRUylL9ckDPqej8zDHqdHfQUPaSr57supLlFMfHkzowmBwNzw3wBbDWRPaSwWSiBweLtACZuLeoiaOpwPpiOhntc4MmCHNfWYsLmIlh2RY7Qbmf%2BgN6dEzhdGWZva&X-Amz-Signature=99cde7bed9ebb103a643878011e66d7194da3e7605d039af3f4a58609a5b9968&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

