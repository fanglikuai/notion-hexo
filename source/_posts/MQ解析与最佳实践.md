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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S7ANN7B7%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T000042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIQDMT0mkuCdKtxFOx1Spcnn3uRfmW6Kkp8cHGh%2Fwwkbb%2FgIgbtQeBw1RgyTlCtPMj4ZgHfCnvrOhmA1QDTTl%2B5iFYEgqiAQI%2Bf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGiFym9BKN84MX5zMSrcAwa%2B%2Bfwgh13I9bKNTGd5t5YYMW%2BbWN4Tp8ES4mEnx35DrWASPCK0bqREoAuPabHvEOfJwAKjhWnDBG%2BtreRrzjjK%2FGZrHkmo%2Bjs0wuwXDeQAqxgaiOSBnpLlVfHkxr9YSw2xniB2ywLQHRYmW8FF3X0Ujlv8dCe0zg7%2By2Lrw6Jwrief69SptaNvON0jeXPxBU9PaEKWqTEXNjSqP7WUvmZl2g0vKHCY%2Fw4XUI4KlyepdG2Azx%2FFVBQokt73KKVU%2FmNDIM4iOb9gZ8uTinPyqa2BhHMZPNdBEIKDy7kJlvMDQ%2BBqRoKVaDcA3riTRooqxNTeTBvoFhzJQZhSzU7rnRJRFH7mUNmpTFNaJp%2Fi%2BFQFm%2BTwPaNn7acD8QXEHnMAnuJZ6jHRvxL4un8IvyAu1eyIg%2BMkKstb7EhOEKr7zNMVVLxQWLg1XqqFtG4irbNlOTge5kLCpLxLxMQf3LtzVsnxrUjUBQyZQAp8KBiuNbMq1HvutDinKn%2BMpSfQfLPoIV9PXW5pCQgTfO3rbLLjEaTvFsdhzd8%2BAmSF41ZwsDnyQ5uGrRjt1oFtdVTLjoJIUqmruIgtN%2BZww88XXNmEPuc9osqgWDrLJ%2BAw6bSYUxqf3xApPDvIYLZ0RmxQMJbxj8gGOqUBquuyyqM3plWFpG7UYHC7nRW1xKfVc38u2dFL8%2F2rNc7WrHizvF0C8FjhPwymSJo7JaS8ElX10TokJKtghOX%2BKkR5%2FaRa7noWCfJZk9kz64Iamf4uYGtUkclz7da%2FdmC%2BRjEqLNgtOzod5Mw9WhsieAcPMhh62hrzd5zElcbP7rn9o5IUY%2BT8%2BxoiTRYQxJ1%2BgjVr6YkIF0S%2BxteLhYM%2FpEYwFSHG&X-Amz-Signature=ed6fa9fc1c000a340c69ea7ca88e6fdf3f98384ff162653b71c63a88c881225d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

