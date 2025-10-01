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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46663CZZGHI%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T050041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHQaCXVzLXdlc3QtMiJHMEUCICrfZ8zPY8ykuJETs6WPqNp69KGYXSkzaZpc%2FfD%2BJGlDAiEAtDrIDwt%2FyxW1HV79put9q25TZhC6PRa8oAT4rteWPwsqiAQI%2Ff%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDD0XqFWxfaS8CzMemSrcA4%2F42vq2JDaoASWOniC38RylKtO52lCojVgNECg8IasMpLnzQeoVCA6FCLAxJ4wy6D9Dj3zuc5zF2jgM%2FxZ1ZwVDz1rnFne6UOZ0z%2BhVpBUcXvcfuehLs2%2FAuCFuCCzr5ZlCoRAXZ3FOb3Pq9WM6K0pFlpLDongHV327ycXqNNJIhbJcuQ78S6aINWjDHDpl2pfyndvxn6s1UV4KOjXQECFBLs2CWZmpbNiVLPmRPBt5QMdCkdDAFaoqG6t4KyZm824lBgiQApVgYrNKL81gUzg2ua%2FK59nM92BO4jJ26pTlIBz9CHg25tKagfRY%2BoV5ByjHw4XD36Ik4zlpZQ%2BYJ4MALFAnRidxCqqMn%2BPbeObnseAS%2FD27t9wq4DwbGcPGQRrspg7JqBV4J75de%2F3QDBLLM4JkaVdGrAGc1pCoFZ%2B%2BhSTw%2F6kmdiuOnSBXdAptSgrYD1dZGKENBVYR3KqNi0tqmKOYwgukoYIGOJJhqrcyaXrs1iWeny6cznApDSDKSP9IVW4X41MAe95HGtCGQ1gWX5UlsUp6oiQgraA20b93ZCn1UO6vz90ZnD6u4zTnIedYAZrAtLIFp%2Bio5XexiAAA4D2raQv%2FJdOR6pC1jrM9DfjFhQAAUqQTUDG1MN7N8sYGOqUB0wC2L779oOzAwe%2Fh49Js9lCtyqg4th3smpMZ7rboVEXlC14FzwhYUkivYA2NSjjBYi7Itm%2BoNhP%2FRSWvBi5w95dxRqznq1aZUI5ko%2BD4t7DxF%2F5%2BGCDcuvhwLDKXtiUOeZ%2BlPD8unPmb8k2bnSsDnJPFmfIoWjsIwuE1KrE8pEaHxb39oOP8iZQwsAyWRZ9xoUOnV%2BRF5AolwyEc1yA63GB7IXuV&X-Amz-Signature=77fecfcd8b56b632e4ba9b9b4615714d967e8b024ae8b7478626329002aee2dc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

