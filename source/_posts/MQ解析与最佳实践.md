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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WBPFUTVR%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJHMEUCIQDNdmYecUQImvRGwdFFV0HlC8XBECQ1oqT2RWGVhgjfVgIgcByKuM3l0Km3OsPW3U6jstAzXO2myQD4zgKG%2F8n4t5wq%2FwMIKxAAGgw2Mzc0MjMxODM4MDUiDBVF0Qk163ZTcG8dQircAzzWvT%2BRA93nJDJbE4vMO3ScN2f2tJVpwq6zlW9C6wg1Q51ocpLXMf8bEb9NfvQUBiIlfe1eSxuCpx%2FhRRwMEBM4sV%2B3TbqSwByhNMzJ4PMd3py3IrPQm6uIOYXrxjMZRYdsxY%2FCmnT39Xcg1b6GEOOcTPAw8lD%2FZgTfFpPQC7CL082oknJbLIsnmU5Tw%2B%2F0028bCtdEJ6vQ30y%2BKj1UnX7NSGcsN8e3ShPinWtj9aq5t4rJm44XsfpY5fref%2FQgfEc%2B7xuHY9CDgdlx%2FG%2FVheNmgem4BitE7tJ13yCHmWKol9EFmnuTSWv74jeKpc0uA5pfRxxGocgexBAQs4W78JZcP8EsDwMLdzF3EuGKmVJlODRoTNUUOkxUvrQ8YZ01%2FmSkOVtG6sJYwUSvyeNSj7Y8hEljYY%2BxLcN84jjZ%2Fi3AgCpQ4tsO18aLIelCC5k4wNwYfhHoUZ%2F7q8xMtFqdtHdo4y2on3zqm%2BSTBipN6WGRszR7RdDyxDM0rOoO%2BMupTEwdIQ1sx0wSQLagMJ3TaEZd8GsqNK6rtQdSNOkw%2F1mCWj2vfWA0QDU3dG8iOFUhjVP%2Bdh0oSAl96Ww0VnGspjUy8LD%2Bu704JTJzBRd1AFZxg9tF4TxQ7vN9t%2FaBMMnMz8gGOqUBiISxLUjfprh1bsLOF9tQZm98jkKBQXTinoyhI2PM%2FlBn%2B72o1HURPTLJSIhA96q3vXOLPOyPyYaaAZI5LKV%2Bo4w0Rl31xV%2BvycdqnIQyj%2FaZPl%2F25qH48BGWML5B1w4tQRqfWSnm1lPdZweGaSqk8xgavjPQemFofqrH42AMIdS%2F%2FmuIw685xkMdIoLKD6o8nvnxjv0gCDXgyLy3sOiTeGSsatC2&X-Amz-Signature=dffd9da288047f85c62a70dd818586548576319f1adedfb5d9d75a7d392b2eed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

