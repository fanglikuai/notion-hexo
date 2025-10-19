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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VB5U35AV%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T100041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJGMEQCIBH1gmHqrmizge6SoPjvoGPUyIhOVA3Y6AVMUbNf6IEFAiAhgpeS2rNPY4CGs68gT37YcTVo00w66Ul4JpxKGSqqcCqIBAjQ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMLkrIwIR5OaICxu%2B%2BKtwDLP9VbSI8dZJLWqvF3iK4U8D49fS5kpbwU8s3pt8XNi2IiM3s7m6K1xsxmJ8y84YR6opES9eUoM1%2B%2F1hgz3nF1VXcQk5WC50DhN8Xg2LY2J7TT5h%2F6iOiQ8EvVMK314IGMx%2BIEfrBYqIPA2fGQWZm9zAcYv5XQrXY0hg0Wi%2FVw7gWY%2F3t3p7znTbLVqWuanCPGgngrZRCUUGs3kAILzRmyJCdyaoinhTl0II9JtHz5VcWC%2FG4XRk71xj2TFiJZISryAZVo2%2FayWAprhlFwJ%2B8OBaVOqUhzFv%2FL3OCKVk%2FWJpW9QPRnYRWwWEON27aKeJVJC8RskwomOb8PlOndz%2BX29r%2F5IHpqmeuHmGUKHK3bpjQlGsjGSyTh2uAY1Wonsb082u0DbkZB4ksw3H%2FjAkFe7XIjtpJI2SaHBAj%2FAft8sGo1oB3P6qKEVnZmo7MKpDuH%2BYj3AKe0JUNJxhjM6lHPszthbBfJKJ2OnV3eq7CxuxrA3OvwKrRzzytRa%2F1oGFwyYzT8thY7%2B7jG3eoobNqw6F1KXJp%2BjEl7xHioUFKKIK2dr7cKtLkjeDun71qoMSInOwIE9jIdvQDD5ofxWVdF8svdSfFWdZo6H%2B69QupR8oyHDlbLfSbuXlAt7wwu47SxwY6pgFfC8MwxKIwgGYCP%2BmUUqjWt1P%2FtYZR%2F3pingzSUBJqPo5H5WOzWrQ1th1ObYlWXuENdJ%2BSIA1c2Tu%2BpzQQrVVCRT878NoSiP1t%2Fhjo8gMWJHeSxaTtVmEyPBUo5CDrPqpMC48xhBwNIA7q%2BK5V18TKdwzM7eX1B1XoBBsxTQarBLUE8xmQEcROi%2B91rEgqbToeDT98UWXTnMc8s0z3fsMgBB0slXyO&X-Amz-Signature=0508943872d4e8272782392d2d6c3c8fb194a04c03f98e7278a625ef9bbdc152&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

