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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3TAQGQA%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T100049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBhHWgHhZo4u7HF%2FE7H5uZYbIWGabXvsjWmQj%2FIxcyWEAiEAyBGu%2FoWkvZpJFJjJzY8%2BZxeaiFhVVcORtZsfQVwv9e8q%2FwMIbhAAGgw2Mzc0MjMxODM4MDUiDGKGkbvxoUaODBP7lircA8O4utnBRFpZqoq4vkXGMtEXzefblyVJ%2F17ZXG46eSOeVECbQGUILTFUs%2BGDVzakFSYuYpw3gDq%2B0LFOJOG9whSGPLFWgvVACV3Voe70trHDweFinpArav65BO1wnRtwxmrztJNKP7HXO1kfnN569%2FkDj2%2FE6wjaycr8IzHNCmXSHdTQF3FbgO%2F1zxtq1EZcLA7tO1gUKImYD6cGN7tD4TWMd7n%2BSZ877F6eBzZDIws7XFhLxcHl7Jz6q7XS8d0BaNJ72gJAivjw8caKLXSs%2B6UxaqyqVOA4XYE6uWZxAXSafgg2TVLT6CieSf6JHnFpHvM9tXfQvZMr0p4CEvLbdfJ55PqJH3HWR9JrDPncQczAF3dp68Xd65z2A%2BnL4S62YV43dVfPUdk5wKL9YbyNfAMCJjV8lgUG4HChd1YJ0wrx74s%2B6Ej6tFwCmczbBe0LG3%2FXFnRMe2p2zSomlvixxuolJI0vtsS3GIqwUuW%2BF0zyxhfx5uYI9s1opF8akiU9LjOaxTmX9q7v9VzA0NIuAamiOzYLYT9ChpofM%2BRN3da7VzXUoMcMPZgRb7e7WSgB2ZqPoA5j0L%2BqBpPEsP1j7DIThDAh6nfwnpgz%2Bj9q4tCn4O5yszcuxH7tFsQ%2FMNX7h8cGOqUBUppSieK2GnrToN9hckIzvgvnx61AhB%2F7GSz3VhM7aeYG%2BGubDKLXkJIEHxn64ty0VW29l7Q2zDY5DWszFgvjuEKMLgR0vyDBZE3k57blt9inrtyrqj23bowbNWBKrgU6KNsaOnN47iCy7aGaxaHMrFU3iX9Ccrq6jhP%2Fvtsh1DLRq7oNtsKIexgYfOEsAWmqnf82Vdn9Cva97WAEK3JN0%2BdeDj8c&X-Amz-Signature=88cf56ae2eade268df99d8827a54bcb4b01029dc3ca92579bfbfd726cf6120bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

