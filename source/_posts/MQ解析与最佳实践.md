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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46653WPTHCA%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T050049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAoiRfPmElrUXaIb5YWQEenxG4e2Ss2anpt6SrvbrZujAiBOaInzAqVtCJEbdMEcJcLCevh6xGYPxgCKhit4MdAhpCr%2FAwhVEAAaDDYzNzQyMzE4MzgwNSIMoiSiu6a0FeF4RTKLKtwDj0RQyO1LCw%2FPMX6m93qPVu51AByG1Ml3CWph3ft5XFGB2vTmie7w7jaqQXVQqCddgXQ%2FYl%2FHSdxFRiahugosXgOSmZawyghZmGK0Bh10fGnZw6TNqPDhcZZxUMZMmGIbAv8MPgiD70UCFFJn%2F5hoC%2BIUiwnZLITrvqr1gd8NCVA09S1TF5Mz%2Fey9kThEEFCv34GSHB4nsi6xETeG7YZ%2FpoUVHQH2EwahdIsQ5f8euZJzpi5n%2BK3y4Ax%2B8yOtIYIoLOX6XKfTYKqJ1g8nlk1h2f4%2BqPVudyw%2FafyQGX7HVFcBYfg0%2Fz7ykg5qKIcp7IUMMJGrph78KKlTXyJnDbU6yt7G7lQyXPYYSel%2F9mUx5QVnLhScF2n1LkEMgL%2BM6qDLlD%2BW7CiN8lLwM2aFtZe%2BIECgIRwvQ362pUYpwdBnx%2BQys1QZCfyIsBSza9dHIhlbI%2BWqfxYT87DBryLNWcox4cZXFcwYgBrTMWVrc9V08HprPx8vnnkehoLF%2BdpofAzyuxgB8RnSLWUnbV1%2BvCtNZRGNzCSknWzG2w0IiqCa2um51wzZ0ARBL5lCSDgU7xe40T%2FMadhB370rAVBY0Th81IjVfL4af1ejn4Gj%2FNMLO2e3S9CEm4vg%2F5%2FffVUwvb%2BCxwY6pgGaUwMrCQ3qJA2hsCY0LTO5IjQQwgYx34b6fc%2FiW7sv%2BvDYQ1Fq4DdE5b44Rs5R%2BFoeSSkkxBX4rduUMv7U2krhgA5t66ua4wOBEm5ou%2BwgYjtePBcZadexUnpk7SsfW%2FmHFobroPsjeGEuXXibtIvzhaNlbzzMwEv2BRn8d2kEzB%2F%2Br0iAjaCBc6VgPNQnsA%2FU4Thm2Qgui7my0YIBJqskt0PwxnSU&X-Amz-Signature=19d1aad00090164566400694d56c16b76f34e2aad44f0dcca06c5b8057c7ece9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

