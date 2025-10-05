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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WFFVRPQ5%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCUImIznjk18P0OLHKvoNLIGL8xZgfGzT%2BOS8iDAOwxVAIhAO3ptfwsGoU6PA4FQ1WpEou6eZL%2FR25AeuIpAhonYoV1Kv8DCHoQABoMNjM3NDIzMTgzODA1IgyxF4QB8XMevybRmHgq3AMOFK4N6BCcmJgovlJmWHNsXEEsM7SKCUDxmgxzP0vRozzYAvTHpN9ZntdOXjgV28De2JqSCQwfRajWlQfPfyN49gJDvdt0mYdZ9BiECqeijiBl9zEN3%2Bd5Zpn4BvB9az2%2Fwsv03XwMsvrb4nizl0xsLBGjRSt%2BbyoChCfWmsSzVbOyEZy5im67ueHYdZfuLifkJM1o1h0M0PUT7kN%2BhrU3w1Kwn8Cx7T%2FmeyXTLuyCu%2FdTk5csCOHpoYoRRTPN8C5EZ8uK5PSQ1C6CqfRvOs%2FCFntdYqiIjfuxJpSOLQlbNXlbZ9hNtzWGyiAqhYkaE00VHhA%2FbJYJpmRLr4BZRCEhDIrEa0Kuuwy%2F2tIdt%2BX5p4xY8H4ZKHvWyUSe7H3dlDmTXrxrPx5%2FEPad98CHG2v2eErHWuoU0JuW%2F1h%2BRzC7QD217Mq0%2BV0reiWS%2FihyP%2BjEan4e7d8r6RFjjniBAKn8k5Q7Qm91H3sa6yLDsMWjFmFhQPZX%2Br6k5CpIhvwmUSIVFtF1FROEOnZ%2BUaAV0bdDa15MWx51vEymn8blkYQjkZcBWNUPi4fCnjfwY%2BikJtLkyyVsZLJLyzQOXlu%2F4RO3blotFp%2BMxcBpE%2BA%2FZe20Zr4ZbBBol2WXWg39xTC%2BwIrHBjqkAT3tiFqvnRYlHIKuX4zNbaDEC%2FL6HVXPMOdgtBvRXJIf%2FsQGRDhSuJSHqbo%2BeHsngLM28wLhbTqzC9RW%2B%2F%2BPq5PcsdJN0RwtMVxOa3aYXAaYkHhRgT55Yylfc1A6VW49SsvejBbSntTkwrlRVidjBoDOblsnBJYIcuGnq%2FHtQrSYszYXhw9DfjstOLH3BLuvx%2BAsbUFyZvRMwKVn8RQ0IsF15pir&X-Amz-Signature=d61251a0c960a1b9f75af5fc5090f1261007bbd22e89f670c76daf3b7f12c786&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

