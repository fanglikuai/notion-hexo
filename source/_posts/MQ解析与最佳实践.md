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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q3IMNLXF%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T150054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJHMEUCIQCAjzIBGxq2dN2An34QiCNvbnOcTHBgnytYZcAkQy%2FfMQIgbYyAaGp6XEkFZO2aADNtsPeFcierZ8mYj2ZYuq6Qp%2BQq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDN92YpHRllxXQLzsRSrcA7xICYCa1HxYIg5LPn2lyJwojsrkiJIxeDgjbpCDgfxp3isCVpMvNe5VNXOIALh3lYCZlTqN%2BmJSX%2FC2IN%2FSVRTj%2B1idZ%2F0IEH3UHmSooLT1MDoMzTrKh3SHAyYmEyRkPT9r3q3%2F6fL9KOIvRmbRZ7YnkrkXOVs58KxAZkkss8TVvZRZ0PNIfEKnF%2FAP8HdX%2Bj7URcBOvGh42trZ1dZKwq0rgGYywgG5CHrwXOSNECYaKol1oflj8CQEMeEsjXSDyPVe7aer9zTfqHtUPEwPSiRcSOB62m30HAaXRBp5TeYL%2Fefy4tjfrN0RRVApdzP53C8kQ7s80d6J4lFsdLZVbooh%2BxRNbuyfzuXYaoL%2Fek15WXj9ynvbSjVPn2T67btEC%2FmSbYpFzIeOeQEz3e6kHapHL%2Bf8rF8GfFZ0tEqor0QuZ0xXipVadrkNVDQF43%2FVv%2FZJtfDHJ%2Fia7%2FwbFmk6lb0N7Y9ssBN3rTzofEmWGq34PFFG9z%2Blwmo1TF20rGjgnzO0QKzV%2Bkw1WzUh%2Fu2xBzmfeRlLUbBUrtXB7RPF9tQNwR4nCuZL3S00hRcRBTkc55CmSjCTrsFuqgCoG0IXYoFEHlyKB6TewEQFnoDDO1WV0r5xLlb4kIfp%2FiC1MNH2l8gGOqUB8TPDMgUks2RfsC%2Fm5uwAayyFouHN5T1%2FGYc4FRciiNtkwjNA%2Fu7o3qG4GEkhJprrAapyzb%2FCdGjIhZRkLeWC6wwc0XdoXi6x0ZWuo1cPZec70gkg2ZleFGvqhCZIqKrf7dsuTlOLTSP21ouIZGiNm9sbk2Lmy1kDZMpB4jXl5N5iTC9DqNsMPie%2BkV%2BZXKnpCA0k5JmWxZanotK1eT9K1HEvU4Ek&X-Amz-Signature=04f56eb37b83954788c40505d2f48c1d65bd32580f53a6e74cf9d72ebd8fd37c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

