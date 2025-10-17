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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662AHOA4ST%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T100048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGgZB4sJMz4a4W9rZPYIpzQONwTaOyLhov2ORC%2B%2FtL9HAiBUGY8Rp6ml1MOP0mFWQ%2FjnDVWG3V4ASU2QVdC6QWTFpyqIBAih%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2F5zr%2BqGSW7nbCzb8KtwD01jvyvFAiag7O9jNt3W%2FWeJ5x6QpEOU2IZ4om8o0uvysuYSswJfQhEyVs%2FHcCbkIDCbNWaP8R7Cy1DaWILFIvx2DCSX6%2B2ReJqsistPDpeYIndUyDthXglsw1n71S5RGAtZIZG8KXN3x5qh0oyjDeebH1LxrxfvHHVYkPfONLHrzprxft7YVhktGrebOv2wzJq3giBC2Br9wYn8RyL7sbKLHoPwMubga8TLtwMNnE0jqi0cUMGQuHW5N1yy3t67zeBbroCH%2BuiwUzxS9raTNS2uNgxE5%2BLUGI6uOHz7yvJipAyM6bL2sZ1KzILb7Ehqj9o8OsdUz5NnkyNPWlHlLHixFBJWhPX2%2Fq8Y5wGQdW9ruBvGhXmhxW4BuSpgoIqTFe4AzMM4QvD%2B01oi0h0bY3nM6nEDe3zqXBSGuwI1drqkRif2Dh2luQ6UwtM%2F%2FMKugl9geOXhIOJGztHlpcZCMRd998%2F1NlTqhn3D7fkLZq63%2BhLgy4a69BdPpQOg0JbySia2M79rHJf1KpyfBVYusYoJVRjUauy%2BzCuJCRmhKw1b2eC8b74MO5BJi6JD2bsv4lm5ToAtqt6ZDOHPFhArsu1LzKAkipVYog4lYWS2fL5Jqyjher7BRQu9X4GswnObHxwY6pgGCR1BZiP0FqGxEE5Ezim6Wq7sAGSMFMXR1XhDIbNj0FJa9iBtC4ast%2F6S4E0UF2nMNy4S3sk3s1fGr5drZfVy9yG93MS8PmLQ77MQyzcMDLDMsxmdw2Cw0Mfrk5RgkkiXDDxQqCMRVnDl2bbjBo7gv7STwr%2BTRggK9pNKLATar56BeG2e%2BDxP%2BmfMkPkTaZ5K1cAXaWCqjIw6ZgqvwscgBMFtW7Tsq&X-Amz-Signature=fc2768867b44a9f482008cff1462b8d0f7b04c8dbc90d2d23c1fe63ecfb9341b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

