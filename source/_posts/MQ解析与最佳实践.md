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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UFLCBMRT%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T120047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIQCGsUeCXl4%2FAm3uEZef0xFWzp9Z4Z2jTqOMAjrViVRlZgIgQ%2Bu0LNhVYKSnxiDcS1IQR0v%2BZX7Sb7uOZ%2F79CCFVF5Yq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDMhUtp3iCNtsxdUJ%2BircAydM4ozFaywhs%2FkObDjm5T3%2FK7oZM%2FibX6r94arFs6pNgOI40zu5%2F2%2F1aK4geE6rrBE0o3GMZRJFgz9SN5Xjck44C%2Boym4EfRbIzxoW5TPBtlvHtfJcqDW0mjIdmBookr1Xli72n%2B5wVgMRJtNG%2Ba1qRS5VjCF7DCk0MH206Bo%2BfunAS1GwasSMLnjxNmfBwC1z2ERWKbjyStv0%2BHLHrdjFuXFVR83Mh8eFo1u3hK8HjuCjU%2FxE8ErhUvk49VCAuyHTC9hhUciuMTWdfQpguRaEF1GT4%2F%2BTK5beiLJ4sRkZVCOuKb6gKtgOYcV49ocB7tdG%2BHk8NZ5le7kpEp3pqFXP1AcUh5%2Fx0HWemIXl5c7N3RXCVvZeoxIRX0vtq2%2BUyoxdKvI6SYbx86lE2dnLAcQBHRnlLB0v95qUGuDrVEaqEH%2BLxYeZn2X%2BbwI6%2BWTAOEoGt71kGzjmh1u%2Fq2MwHX%2FluColN2SuYfnb5nKTXmohQdMAEUvf8TPcRMihZP0AS5yZgZ7wXR3XIc6X9fP%2FkLLE2TPiD6qMWLEOIAMoqhSQ%2B%2BpcC%2Bi%2FOdMdKCAROXoSMMgOf9lDy4IKd7E%2F4GtQVU9R4VPd5w4xZAlSBNWjJBF8rCnXDOte97Zn5KtxIMNaXi8kGOqUBQgeIdgbEgm02TY7FmJh0cGbyPvdvtBS%2Fj1J59mfIWBK5Hmu4N3UiXhevw%2F27iKVblisDtR6zGLc0AXZ0nt7ERfOLPzZQOi1J%2B7T%2FyZdN%2BX0Pj4V5KKlZLRsvhJOHQGPOdiRBcUMBcL2UmVwcYRh%2B8lWo6zE4SRDCZpysuRZHMnYnQqy4XA52%2BrdfaQqrJ%2FaJr2yuhltU9bxpqLmjbfpSoF6kBoH0&X-Amz-Signature=9050d25df06c588040bf129ea546bc215f11087d1b4272925ebcf3a5e1198c44&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

