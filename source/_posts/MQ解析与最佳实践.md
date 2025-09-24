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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QQJWRFKS%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T010041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDysqupAJB35FtAxrn8ZgDAC6ej8wjvNGGPNl4HlINgmgIgafv6%2FzTC07Zm2fPCyKwxsI1N7XnOd0udMK%2Bbeap%2BNBgq%2FwMIURAAGgw2Mzc0MjMxODM4MDUiDCCuBTFxbFysILPS4yrcA1uliV2mxjkrbdfI%2BzQ325sSCEMUeR%2BsW%2FrDt1PZVuEdSE4jUquqBB6hqbUHwJThgPwgUQIOSFaWtjugA8Lx6vc9cZC%2Bi4Ixj24svOMIKjBIMzjpiyIYkHPfZMtPfeQRqq1u4KKIW3p%2FdY1qRNy%2BjriHAWcTWnvZQ4GG%2FyOvoisiE8xxiYRyR6dyzwazOFKtYXKjDWnWbF4PybuQScUwEzgGLU3j0xt5neruJEyFUH3A0P0XRjy0%2B7Pd08dK%2FG3tYXMkEZG2dbKXPNKDUmqo40Ki9hg6nXVPYJEg%2FkgrQHe%2FT0fszAjRA37p%2BZYt0gubIlNB1j9adm6ivWhpGefbkzheWZkN3hpAmasOX8VlN6lgnxdBjHdvP4siylEx9n%2Fm6n3Tpz8sJrUGy8kA1Pl6gIcgB9OyACMWyag9QdwE%2Bsrnqy4ChrnK1kb1brlZyibbuhS3Hc6sYaJi5Mez37t%2BmLHtGxsQyk2x6dKsLa5YSeMuRYdOWLcgzW1M1aLH9Ov22%2BVX1u%2FlOcfm9O85MoAMAdrsjMq%2B7zIBSKF0NDw5E%2F3GvVvwTRFEmBHTHFthMkO%2BSUsx%2FZSOUfiU%2B7pCdoqRAVDOVBawHzbzUw%2B%2FXYHrkHJ6YXF%2F%2B%2FSwXzLpM0pSMMbnzMYGOqUBt4L8oyNfSMkNqCUG55kcYQTkQP2QuADHfEQn%2FFiFTxLMQ6%2FnY%2FgYM1cUT0W%2FNL4e8FPZnCqG27FjrejiGsSdTuLruMtpCqW83Pu8AWK7uB7jcWT9Lj%2BYlv9We%2FHJWtrs6g%2FX9fjsM7Cp6ZSC4lFtIeeLjcgfjHDc%2FwLAsr4W%2FzuB8Bh%2BONIm1SNLbXFDgIyUEe8AOQkmkF%2Fc1XchzyTce8qDZJBP&X-Amz-Signature=48abd667bae6ceceed551d568b660daa005ac8fb114b1b2546c19185183040a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

